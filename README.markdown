JoeQuery's Rails Template
=========================

This rails template is the product of many hours of my sweat and tears. Much 
time went into Googling and debugging to properly configure crucial gems such as
spork and capistrano. This template aims to save you time and save your sanity.

Features
--------
* Spork, rspec, and guard preconfigured for a fast testing environment
* A well documented Capistrano deployment file to make Git deployment effortless
* An example Unicorn file for use with Nginx
* An example nginx configuration file


Grabbing the template/creating your Rails App
--------------------------------------------- 

If you are using this template, you are no longer using "rails new project".
Thus, we need to grab the repo, install the gems from the Gemfile, and then
rename the project using the ```rename_to``` plugin. Then rename the 
Rails-Template folder created when you cloned to repo to whatever you want.
  
```
$ git clone git://github.com/joequery/Rails-Template.git
$ cd Rails-Template
$ bundle install
$ bundle exec rails g rename_to YourAppName
$ cd ../ && mv Rails-Template YourAppName
```

View the wiki!
--------------

Details on implementing the features of the template can be found
[in the wiki](https://github.com/joequery/Rails-Template/wiki)
