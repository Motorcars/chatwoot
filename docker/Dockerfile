FROM ruby:3.1

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs libvips git

# Set working directory
WORKDIR /app

# Install gems
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install

# Copy the entire app
COPY . .

# Install JS dependencies
RUN yarn install --frozen-lockfile

# Build Vite assets
RUN RAILS_ENV=production bundle exec vite build

# Precompile Rails assets
RUN RAILS_ENV=production bundle exec rails assets:precompile

# Prepare the database (run migrations and seed initial onboarding data)
RUN RAILS_ENV=production bundle exec rails db:chatwoot_prepare

# Start the Chatwoot server
CMD ["bundle", "exec", "rails", "s", "-p", "${PORT:-3000}", "-b", "0.0.0.0"]
