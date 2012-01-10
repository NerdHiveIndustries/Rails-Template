JoeQuery's Rails Template
=========================

This rails template is the product of many hours of my sweat and tears. Much time went into 
googling and debugging to properly configure crucial gems such as spork and capistrano. This
template aims to save you time and save your sanity.

Features
--------
* Spork, rspec, and guard preconfigured for a fast testing environment
* A well documented [Capistrano](https://github.com/capistrano/capistrano/wiki/Documentation-v2.x) deployment file to make Git deployment effortless
* An example [Unicorn](https://github.com/blog/517-unicorn) file for use with Nginx

Usage
-----

### Configuring Bundler
This template was built using my opinion that gems should rarely be installed system wide (or even
in Gemsets you RVMers) and should instead be installed via bundle into the project directory. Thus,
we will be using ```bundle exec``` as a prefix for most commands.

To set up bundle to install gems in the app directory, do the following:  
```
$ sudo mkdir ~/.bundle
$ sudo vim ~/.bundle/config
```

The contents of ```~/.bundle/config``` will be  

```
BUNDLE_PATH: vendor/bundle
```


### Grabbing the template/creating your Rails App
We need to grab the repo, install the gems from the Gemfile, and then
rename the project using the ```rename_to``` plugin. Then rename the Rails-Template folder
created when you cloned to repo to whatever you want.
  
1. git clone git://github.com/joequery/Rails-Template.git
2. cd Rails-Template
3. bundle install
4. bundle exec rails g rename_to YourAppName
5. cd ../ && mv Rails-Template YourAppName

### Using Guard, Spork, and Rspec
