# Use official Ruby base image
FROM ruby:3.2

# Install dependencies
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client yarn build-essential libvips

# Set working directory
WORKDIR /app

# Copy Gemfiles first for caching
COPY Gemfile Gemfile.lock ./
RUN bundle install

# Copy the rest of the application
COPY . .

# Precompile assets (if using Rails with webpacker or jsbundling-rails)
RUN bundle exec rake assets:precompile

# Run DB migrations automatically before starting server
CMD bundle exec rails db:migrate && bundle exec puma -C config/puma.rb
