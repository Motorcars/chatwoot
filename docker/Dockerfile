# Base Ruby image
FROM ruby:3.2

# Install system packages
RUN apt-get update -qq && \
    apt-get install -y build-essential libpq-dev nodejs yarn postgresql-client libvips

# Set working directory
WORKDIR /app

# Copy Gemfiles and install gems
COPY Gemfile Gemfile.lock ./
RUN bundle install --without development test

# Copy application files
COPY . .

# Precompile assets (if needed)
RUN bundle exec rake assets:precompile

# Run DB migrations and start the server
CMD ["bash", "-c", "bundle exec rails db:migrate && bundle exec puma -C config/puma.rb"]
