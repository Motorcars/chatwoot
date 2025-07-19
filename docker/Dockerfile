FROM ruby:3.2

# Install essential system packages
RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs yarn postgresql-client libvips

# Set the working directory
WORKDIR /app

# Copy only Gemfiles first (for Docker caching)
COPY Gemfile Gemfile.lock ./
RUN bundle install

# Copy the full app
COPY . .

# Precompile assets (skip if not using Rails assets)
RUN bundle exec rake assets:precompile

# Run DB migration and then start the app
CMD bundle exec rails db:migrate && bundle exec puma -C config/puma.rb
