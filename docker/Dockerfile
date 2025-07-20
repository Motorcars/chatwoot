FROM ruby:3.2

# Install system dependencies
RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs yarn postgresql-client libvips

# Set working directory
WORKDIR /app

# Install Ruby gems
COPY Gemfile Gemfile.lock ./
RUN bundle install

# Copy all app files
COPY . .

# Precompile assets (optional if using Rails frontend)
RUN bundle exec rake assets:precompile

# Run database migrations and then start the server
CMD bundle exec rails db:migrate && bundle exec puma -C config/puma.rb
