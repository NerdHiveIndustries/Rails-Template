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
in Gemsets you RVMers) and should instead be installed via bundler into the project directory. Thus,
we will be using ```bundle exec``` as a prefix for most commands. Don't worry, the template comes with a .gitignore that will ignore the installation directory of the gems. 

To set up bundler to install gems in the app directory, do the following:  

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
  
```
$ git clone git://github.com/joequery/Rails-Template.git
$ cd Rails-Template
$ bundle install
$ bundle exec rails g rename_to YourAppName
$ cd ../ && mv Rails-Template YourAppName
```

### Using Guard, Spork, and Rspec
This template comes with Guard, Spork and Rspec preconfigured for quick testing. To use the testing environment, you simply:  

1. cd to YourApp directory
2. In one shell session (consider opening a new tab in iTerm), execute  
``` bundle exec guard ```  
3. In another shell session, execute the call to rspec.  
``` bundle exec rspec spec/ ```

Since we're using bundler and installing gems in the project, using ``` bundle exec ``` is necessary.

