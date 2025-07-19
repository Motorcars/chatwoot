FROM ruby:3.2

# Install required packages
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev nodejs postgresql-client yarn libvips

# Set working directory
WORKDIR /app

# Copy project files
COPY . .

# Install Ruby gems and Node packages
RUN gem install bundler && bundle install
RUN yarn install --check-files

# Precompile assets for production
RUN RAILS_ENV=production bundle exec rails assets:precompile

# Set environment
ENV RAILS_ENV=production

# 🔧 Add the required CMD to start server & run migrations
CMD ["bash", "-c", "bundle exec rails db:prepare && bundle exec puma -C config/puma.rb"]
