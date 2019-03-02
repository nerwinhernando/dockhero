# README

This README would normally document whatever steps are necessary to get the
application up and running.

The content is credited to 
https://github.com/gabrieljoelc/herocker/blob/master/PITCHME.md

Added a snippet to generate rails credentials because without this, the deployment to heroku won't work. I am also planning to create an rails api only generator for this.

# Prerequisites
Install Docker
Install Ruby
gem install orats
Install Heroku Toolbelt

# Build Image for Local
* Terminal 1
```
> cd ~/dev
> orats new dockhero
> cd herocker
> git init && git add . && git commit -m "Scoffald from orats"

> docker-compose up --build
```
Migrate the database in T2.
Compile the assets in T2.

* Terminal 2 (T2)
```
> docker-compose exec website rails db:create db:migrate
> docker-compose exec website rake assets:precompile
> curl -o config/puma.rb https://gist.githubusercontent.com/gabrieljoelc/8c04941042e9241a41b840cccf1ad5fb/raw/puma.rb
> git add . && git commit -m "Split BIND_ON from PORT for puma binding"
```
Create a new environment variable PORT and set a value (3000)
```
> vi .env

> docker-compose up --build
```
# Deployment for Heroku
```
> curl -O https://gist.githubusercontent.com/gabrieljoelc/8c04941042e9241a41b840cccf1ad5fb/raw/Dockerfile.web
> git add . && git commit -m "Add production Dockfile"
> heroku create
> heroku addons:create heroku-postgresql
```
Make sure that your app is running before executing config:set
```
> heroku config:set \
SECRET_TOKEN=`docker-compose exec website rails secret` \
ACTION_CABLE_ALLOWED_REQUEST_ORIGINS=`heroku apps:info | grep "Web URL:" | sed -e 's/^Web URL:        //'` \
RAILS_SERVE_STATIC_FILES=true

> docker-compose exec website sh
```
Inside the docker container:
```
/app # EDITOR=vim rails credentials:edit
```
 This will output the following:
 ```
 Adding config/master.key to store the master encryption key: 917983f0528c05aee7d460742f021925

Save this in a password manager your team can access.

If you lose the key, no one, including you, can access anything encrypted with it.

      create  config/master.key

Ignoring config/master.key so it won't end up in Git history:

      append  .gitignore

New credentials encrypted and saved.
/app # exit
 ```

# Heroku Container Registry
```
heroku container:login
heroku container:push -R
```

# Reload the schema
```
heroku run rails db:schema:load

heroku open
```
# Heroku Link
```
https://gentle-basin-51816.herokuapp.com/
```
# References
> https://github.com/gabrieljoelc/herocker
> https://github.com/nickjj/orats
> https://waiyanyoon.com/deploying-rails-5-2-applications-with-encrypted-credentials-using-capistrano/
