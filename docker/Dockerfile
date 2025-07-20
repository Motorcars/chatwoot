FROM ruby:3.2.2

# Install system dependencies
RUN apt-get update -qq && apt-get install -y \
  build-essential \
  libpq-dev \
  nodejs \
  curl \
  postgresql-client \
  yarn \
  imagemagick \
  libvips

# Install bundler
RUN gem install bundler

# Set working directory
WORKDIR /app

# Set environment variables (use ENV for proper asset builds)
ENV RAILS_ENV=production \
    NODE_ENV=production \
    RAILS_LOG_TO_STDOUT=true

# Copy Gemfiles and install gems
COPY Gemfile Gemfile.lock ./
RUN bundle install --without development test

# Copy project files
COPY . .

# Install JS dependencies (if using yarn for webpacker etc.)
RUN yarn install --frozen-lockfile || true

# Precompile assets
RUN bundle exec rake assets:precompile

# Run DB setup (but allow skipping if DB not yet available)
RUN bundle exec rake db:prepare || echo "Database not ready, skipping..."

# Expose app port
EXPOSE 3000

# Start the app
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
