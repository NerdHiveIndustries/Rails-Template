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

We need to grab the repo, install the gems from the Gemfile, and then
rename the project using the ```rename_to``` plugin. Then rename the 
Rails-Template folder created when you cloned to repo to whatever you want.
  
```
$ git clone git://github.com/joequery/Rails-Template.git
$ cd Rails-Template
$ bundle install
$ bundle exec rails g rename_to YourAppName
$ cd ../ && mv Rails-Template YourAppName
```

Configuring Bundler
-------------------

This template was built using my opinion that gems should rarely be installed 
system wide (or even in Gemsets you RVMers) and should instead be installed via 
bundler into the project directory. Thus, we will be using ```bundle exec``` as 
a prefix for most commands. Don't worry, the template comes with a .gitignore 
that will ignore the installation directory of the gems. 

To set up bundler to install gems in the app directory, do the following:  

```
$ sudo mkdir ~/.bundle
$ sudo vim ~/.bundle/config
```

The contents of ```~/.bundle/config``` will be  

```
BUNDLE_PATH: vendor/bundle
```

Using Guard, Spork, and Rspec
----------------------------- 

This template comes with Guard, Spork and Rspec preconfigured for quick testing.
To use the testing environment, you simply:  

1. cd to YourApp directory
2. In one shell session (consider opening a new tab in iTerm), execute  
``` bundle exec guard ```  
3. In another shell session, execute the call to rspec.  
``` bundle exec rspec spec/ ```

Since we're using bundler and installing gems in the project, using 
``` bundle exec ``` is necessary.

--------------------------------------------------------------------------

Server Configuration
--------------------

If you have never ran rails on a server, this document will guide you in setting
up a rails app running on nginx **step by step**. Basic server configuration can 
seem daunting at first, but this document takes all the Googling and 
head-scratching out of the equation.


### Assumptions
* An Ubuntu server
* Ability to use root permissions
* Comfort navigating the unix environment and using a terminal-based editor.
* Ruby and bundler installed on the server. Bundler needs to be configured
	as instructed by the "Configuring Bundler" section above.

Since we want to check that our nginx server properly runs rails before we worry
about deployment, the first portion of this server configuration guide will 
involve you operating directly on your server. Ensure you have ssh access.

### Step 1: Installing packages
To install nginx and git, execute

```
$ sudo apt-get update
$ sudo apt-get install git-core
$ sudo apt-get install nginx
```

### Step 2: Configuring nginx

Navigate over the nginx configuration directory

```
$ cd /etc/nginx
```

Open the nginx.conf file in your favorite text editor as root

```
$ sudo vim nginx.conf
```

Clear the contents of the configuration file and paste in the following.
The only thing you'll have to change for future project is the root path.

```
worker_processes 1;
user nobody nogroup; 
pid /tmp/nginx.pid;
error_log /tmp/nginx.error.log;
events {
  worker_connections 1024; 
  accept_mutex off; 
}
http {
  default_type application/octet-stream;
  access_log /tmp/nginx.access.log combined;
  sendfile on;
  tcp_nopush on; 
  tcp_nodelay off; 
  gzip on;
  gzip_http_version 1.0;
  gzip_proxied any;
  gzip_min_length 500;
  gzip_disable "MSIE [1-6]\.";
  gzip_types text/plain text/html text/xml text/css
             text/comma-separated-values
             text/javascript application/x-javascript
             application/atom+xml;
  upstream app_server {
    server unix:/tmp/.sock fail_timeout=0;
  }
  server {
    client_max_body_size 4G;
    server_name _;
    keepalive_timeout 5;

		############################################################################
    # Edit this path to point to the 'public' folder of your rails app
		# This feels a little counter intuitive because this root path is NOT the
		# root directory of your rails app, but the public directory of the 
		# rails app root directory. But that's how it is!
		# Don't forget the semicolon at the end of the line!!!
		############################################################################
    root /var/www/testapp/public;

    try_files $uri/index.html $uri.html $uri @app;
    location @app {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_redirect off;
      proxy_pass http://app_server;
    }
    error_page 500 502 503 504 /500.html;
    location = /500.html {
			root /var/www/testapp/public;
    }
  }
}
```


This is the configuration file provided by the creators of Unicorn. The 
commented version of the file can be found at
http://unicorn.bogomips.org/examples/nginx.conf

With nginx installed, it's time to set up the rails app

### Step 3: Creating the rails app on the server

If the directory didn't already exist, nginx will create a directory named
``` /var/www ```. This is the conventional place to display your web content.

We need to change permissions on the directory so we can have write access to it

```
$ sudo chmod -R 775 /var/www
```

Now let's move to the directory

```
$ cd /var/www
```

Now grab this template!

```
$ git clone git://github.com/joequery/Rails-Template.git
$ cd Rails-Template
$ bundle install
$ bundle exec rails g rename_to TestApp
$ cd ../ && mv Rails-Template testapp
```

With a rails app on the server, we're one step closer to having the app
available for the world to see. The next step is to configure unicorn.

### Step 4. Configuring Unicorn
Basic unicorn configurations are taken care of by this template. All you need to
do is edit ```config/unicorn.rb``` where it asks for your APP_PATH. For this 
example, we created the app at ```/var/www/testapp```

(config/unicorn.rb)  

```

...
...

################################################################################
# Adjust your APP_PATH here!!!
################################################################################
APP_PATH = "/var/www/testapp" # NO trailing slash

...
...

```

Similarly to the nginx.conf file, in the future simply edit the APP_PATH to 
point to the root directory of your app.

Everything is just about ready to go at this point: Now we just need to actually
start the nginx server and the unicorn service.

### Step 5: Starting nginx and unicorn
In my short experience with nginx and unicorn so far, these services do not
die very easily, even when you want them to. For example, if you change
the root path in your ```nginx.conf``` file, you need to restart nginx. Before
the change takes effect. However, using the common means of restarting 
(```/etc/init.d/nginx restart```) tends to give me an error that a process is 
already operating on the socket...and that process is nginx! 

So I've put together a few functions that I'll be using to complete this guide.
Append the following to your ```~/.bashrc```


```
# Kill and restart nginx
function restart_nginx(){
  pids=$(pidof nginx)
  if [[ -n $pids ]]; 
  then
    sudo kill -9 $pids
    sudo service nginx restart
  fi  
}

# Kill unicorn
function kill_unicorn(){
  ps aux | grep 'unicorn' | awk '{print $2}' | xargs sudo kill -9
}

# test unicorn process
function test_unicorn(){
  # If config/unicorn.rb doesn't exist, don't bother running anything.
  unicornFile=config/unicorn.rb
  if [ ! -e $unicornFile ];
  then
    echo "Unicorn file not found"
  else
    echo "Starting unicorn..."
    bundle exec unicorn_rails -c $unicornFile -E production
  fi
}

# run unicorn process as a daemon
function start_unicorn(){
  # If config/unicorn.rb doesn't exist, don't bother running anything.
  unicornFile=config/unicorn.rb
  if [ ! -e $unicornFile ];
  then
    echo "Unicorn file not found"
  else
    echo "Starting unicorn..."
    bundle exec unicorn_rails -c $unicornFile -E production -D
  fi
}
```
The difference between the test_unicorn() and start_unicorn() functions is
test_unicorn starts unicorn in the foreground and can be terminated with CTRL-C
while start_unicorn() runs unicorn in the background as a daemon.

We need make sure our shell uses this new ```~/.bashrc```

```
source ~/.bashrc
```

### Hello, world!
Make sure you're in the root directory of the app.

```
$ cd /var/www/testapp
```

Now we're going to test unicorn.

```
$ test_unicorn
```

Now visit ```http://yourserver.com/pages/show``` and you should see the familiar
"Find me in app/views/pages/show". Note that you should not use 
```http://yourserver.com/``` to determine if unicorn is working. nginx will 
serve up the static index.html document located in the public folder of the
app regardless of unicorn's state.
