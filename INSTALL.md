
Installation instructions.

## Install MongoDB/Redis (ubuntu/debian)

Requirements: node.js, mongodb, redis. Also, we recommend
[Hetzner](http://www.hetzner.de/en/hosting/produktmatrix/rootserver-produktmatrix/)
and [OVH](http://www.ovh.com/fr/serveurs_dedies/) for dedicated
server - very good price/quality ratio.


### MongoDB

http://docs.mongodb.org/manual/tutorial/install-mongodb-on-debian-or-ubuntu-linux/

Follow instructions on the link above. Then edit `/etc/mongodb.conf`,
and add `bind_ip = 127.0.0.1` to the start.

    restart mongodb

If you don't use db auth, no more actions needed. If you plan to use
it, create database and set login/password.


### Redis

https://launchpad.net/~rwky/+archive/redis

    sudo add-apt-repository ppa:rwky/redis
    # sudo add-apt-repository ppa:chris-lea/redis-server
    sudo apt-get update
    sudo apt-get install redis-server

Edit `/etc/redis/redis.conf`, restrict listening to `127.0.0.1` only.

    restart redis-server


## Other dependencies

You need GraphicsMagick.

For ubuntu:

    sudo apt-get install graphicsmagick

You also need Cairo to generate identicons. If it's not installed (for example,
on a server), read instructions for [node-canvas](https://github.com/Automattic/node-canvas#installation)
package.

For ubuntu:

    sudo apt-get install libcairo2-dev libjpeg8-dev libpango1.0-dev libgif-dev build-essential g++


## Install node.js

Version 0.10+ required.

See [developer's manual](https://github.com/nodeca/nodeca/tree/master/docs/developer-setup)


## Install nodeca


### Bleeding edge, server (read-only)

Select this, if you like to see demo:

    git clone git://github.com/nodeca/nodeca.git nodeca
    cd nodeca
    make pull-ro

`./etc` folder contains simple upstart config for fast deployment. It's not
secure, but it allows you to easily install and switch node versions.
That's convenient for demo/development.


### Development (read/write, core team)

Select this, if you are in the core development team, and have read/write
access to nodeca repos:

    git clone git@github.com:nodeca/nodeca.git nodeca
    cd nodeca
    make pull


### Regular install (production deployment):

    npm install nodeca

(*) That won't work right now, since we didn't release anything yet.


## Configure

Copy **ALL** `*.example` files in root config folder to `*.yml`:

```bash
cp config/application.yml.example config/application.yml
cp config/database.yml.example config/database.yml
...
```

**Don't** touch files in `./config/examples` subfolders. Those are for
educational purposes only.

Edit `application.yml` and `database.yml` to fit your environment.
For development no changes are usually needed.


## Init database

Apply migrations:

    ./nodeca.js migrate --all

If you need test data, apply seeds. List available ones:

    ./nodeca.js seed

Then you can choose seeds to apply using their numbers:

    ./nodeca.js seed -n <NUMBER_1> -n <NUMBER_2>


## Run

In a terminal (break with ctrl+c):

    ./nodeca.js server

Run with monitor (your dev location, will be auto-restarted when you make
changes):

    make dev-server

Run on a server (via `upstart`, check a script from `./etc` subfolder):

    start nodeca

Other commands:

    ./nodeca.js -h
