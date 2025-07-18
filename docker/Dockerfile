FROM ruby:3.1

# Install dependencies
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs libvips

# Set working directory
WORKDIR /app

# Install bundler and dependencies
COPY Gemfile Gemfile.lock ./
RUN gem install bundler && bundle install

# Copy application code
COPY . .

# Precompile assets
RUN bundle exec rake assets:precompile

# ❗️ IMPORTANT: Run DB setup only once here
RUN bundle exec rails db:chatwoot_prepare

# Start server
CMD bundle exec rails s -p ${PORT:-3000} -b 0.0.0.0
