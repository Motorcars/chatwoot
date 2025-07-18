FROM ruby:3.1

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs libvips git

# Set working directory
WORKDIR /app

# Install bundler and gems
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install

# Copy app code
COPY . .

# Install JS deps & build assets
RUN yarn install --frozen-lockfile
RUN RAILS_ENV=production bundle exec vite build
RUN RAILS_ENV=production bundle exec rake assets:precompile

# Run DB setup (optional for first deploy only)
RUN RAILS_ENV=production bundle exec rails db:chatwoot_prepare

# Start server
CMD bundle exec rails s -p ${PORT:-3000} -b 0.0.0.0
