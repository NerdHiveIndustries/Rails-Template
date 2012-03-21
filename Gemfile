source 'http://rubygems.org'

gem 'rails', "3.2.2"
gem 'sqlite3'

# Gems used only for assets and not required
# in production environments by default.
group :assets do
  gem 'sass-rails',   '~> 3.2.3'
  gem 'coffee-rails', '~> 3.2.1'

  # See https://github.com/sstephenson/execjs#readme for more supported runtimes
  # gem 'therubyracer'

  gem 'uglifier', '>= 1.0.3'
end

gem 'jquery-rails'

# To use ActiveModel has_secure_password
# gem 'bcrypt-ruby', '~> 3.0.0'

# To use Jbuilder templates for JSON
# gem 'jbuilder'

group :development do
  gem 'rspec-rails', '~> 2.9.0'
  gem 'guard-spork'
  gem 'guard-rspec'

  # Git deployment
  gem 'capistrano'
  gem 'capistrano_colors'

  # To use debugger
  # gem 'ruby-debug19', :require => 'ruby-debug'
end

group :test do
  gem 'rspec', '~> 2.9.0'
  gem 'spork', '~> 0.9.0'
  gem 'webrat', '~> 0.7.3'
end

group :production do
   gem 'unicorn'
end
