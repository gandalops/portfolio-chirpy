FROM ruby:3.1

# Install dependencies
RUN apt-get update -qq && apt-get install -y \
  build-essential libpq-dev nodejs

# Install Jekyll and Bundler
RUN gem install jekyll bundler

# Set working directory
WORKDIR /workspace
