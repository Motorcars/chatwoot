# Use stable Ruby version compatible with Chatwoot
FROM ruby:3.2

# Install essential packages
RUN apt-get update -qq && apt-get install -y \
  build-essential \
  libpq-dev \
  nodejs \
  postgresql-client \
  yarn \
  curl \
  libvips

# Set working directory
WORKDIR /app

# Cache bundler dependencies
COPY Gemfile Gemfile.lock ./
RUN gem install bundler -v 2.4.22
RUN bundle install -j4

# Install JavaScript dependencies
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

# Copy rest of the app
COPY . .

# Precompile assets for production
RUN RAILS_ENV=production bundle exec rake assets:precompile

# Expose port (Puma default)
EXPOSE 3000

# Run migrations and start the app
CMD ["bash", "-c", "bundle exec rails db:prepare && bundle exec puma -C config/puma.rb"]
