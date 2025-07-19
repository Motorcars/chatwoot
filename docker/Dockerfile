FROM ruby:3.1

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs libvips git

# Set working directory
WORKDIR /app

# Install gems
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install

# Copy app source
COPY . .

# Install JS deps
# Set higher memory limit before build
ENV NODE_OPTIONS="--max_old_space_size=3072"
RUN yarn build

# Build Vite assets
RUN RAILS_ENV=production bundle exec vite build

# Precompile Rails assets
RUN RAILS_ENV=production bundle exec rails assets:precompile

# Prepare DB (runs migrations and seeds onboarding data)
RUN RAILS_ENV=production bundle exec rails db:chatwoot_prepare

# Start Chatwoot server
CMD bundle exec rails s -p ${PORT:-3000} -b 0.0.0.0
