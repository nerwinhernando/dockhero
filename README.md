# README

This README would normally document whatever steps are necessary to get the
application up and running.

# Prerequisites
Install Docker
Install Ruby
gem install orats
Install Heroku Toolbelt

# Build Image for Local
* Terminal 1

> cd ~/dev
> orats new dockhero
> cd herocker
> git init && git add . && git commit -m "Scoffald from orats"

> docker-compose up --build

Migrate the database in T2.
Compile the assets in T2.

* Terminal 2 (T2)
> docker-compose exec website rails db:create db:migrate

> docker-compose exec website rake assets:precompile


curl -o config/puma.rb https://gist.githubusercontent.com/gabrieljoelc/8c04941042e9241a41b840cccf1ad5fb/raw/puma.rb
git add . && git commit -m "Split BIND_ON from PORT for puma binding"

vi .env

docker-compose up --build

# References
https://github.com/gabrieljoelc/herocker
