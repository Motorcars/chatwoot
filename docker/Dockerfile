FROM ruby:3.2.2

# Install dependencies
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client yarn imagemagick libvips

# Set working directory
WORKDIR /app

# Copy Gemfiles and install
COPY Gemfile* ./
RUN gem install bundler && bundle install

# Copy project files
COPY . .

# Precompile assets and run DB setup
RUN RAILS_ENV=production bundle exec rake assets:precompile
RUN RAILS_ENV=production bundle exec rake db:prepare

# Expose port
EXPOSE 3000

# Start server
CMD ["bundle", "exec", "puma", "-C", "config/puma.rb"]
