source 'http://rubygems.org'

gem 'rails', "3.0.1"
gem 'sqlite3-ruby', '1.3.2',  :require => 'sqlite3'

group :development do
  gem 'rspec-rails', '2.2.1'
  gem 'guard-spork'
  gem 'guard-rspec'

# Git deployment
	gem 'capistrano'
end

group :test do
  gem 'rspec', '2.2.0'
  gem 'spork', '0.9.0.rc7'
  gem 'webrat', '0.7.3'
end

group :production do
# Serving with nginx
	 gem 'unicorn'

end
