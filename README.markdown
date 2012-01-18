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
We will create a bash function to automate this process. Append the
following to your ```~/.bashrc``` file:  
  
```
# Create a new rails app from JoeQuery's Rails Template.
# arg1: appName
function rails_template(){
	appName=$1
	printf "\n##############################################\n"
	printf "# Cloning template\n"
	printf "##############################################\n"
	git clone git://github.com/joequery/Rails-Template.git

	printf "\n##############################################\n"
	printf "# Renaming directory\n"
	printf "##############################################\n"
	printf "Renaming Rails-Template directory to %s...\n" "$appName"
	mv Rails-Template $appName
	cd $appName
	printf "Current working directory has changed to %s...\n" "$PWD"

	printf "\n##############################################\n"
	printf "# Installing gems via bundle\n"
	printf "##############################################\n"
	bundle install

	printf "\n##############################################\n"
	printf "# Renaming rails appp\n"
	printf "##############################################\n"
	printf "Renaming rails app to %s...\n" "$appName"
	bundle exec rails g rename_to $appName
}
```

Now reload your ```~/.bashrc``` into your session so we can use this 
function.

    $ source ~/.bashrc

Now, to create a new app from this template, simply execute

    $ rails_template YourRailsApp

View the wiki!
--------------

Details on implementing the features of the template can be found
[in the wiki](https://github.com/joequery/Rails-Template/wiki)
