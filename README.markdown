Rails Template
=========================

This rails template is the product of many hours of sweat and tears. Much
time went into Googling and debugging to properly configure crucial gems such as
spork and capistrano. This template aims to save you time and save your sanity.

Features
--------
* Spork, rspec, and guard preconfigured for a fast testing environment
* A well documented Capistrano deployment file to make Git deployment effortless

Grabbing the template/creating your Rails App
---------------------------------------------

If you use this template, you will no longer run "rails new project". Thus, we need to grab the repo, install the gems from the Gemfile, then rename the application. We will use a bash function to automate this process. Append the following to your ```~/.bashrc``` file:

```
# Create a new rails app from the Rails Template.
# arg1: app_name
function newrailsapp(){
  if [ "$#" -eq "0" ]; then
    return
  fi
  app_name=$1
  echo -e "#\n# Cloning template\n#"

  git clone git://github.com/NerdHiveIndustries/Rails-Template.git $app_name

  cd $app_name

  echo -e "Current working directory has changed to $PWD"

  rm -rf .git
  rm -f Gemfile.loc

  echo -e "Installing gems via bundler"

  bundle install --binstubs

  echo -e "#\n# Renaming rails app to $app_name\n#"

  bundle exec rails g rename_app $app_name
  rm config/initializers/rename_app.rb
  rm lib/rename_app.rb

  echo -e "#\n# Creating git repository and adding files\n#"

  git init
  git add .
  git commit -m"Initial add"
}
```

Now reload your ```~/.bashrc``` into your session so we can use this
function.

$ source ~/.bashrc

Now, to create a new app from this template, simply execute

$ newrailsapp YourRailsApp
