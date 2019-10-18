<p align="center">
  <a href="https://github.com/Arbarwings/devpot">
    <img alt="DevPot" title="DevPot" src="logo.svg" width="250">
  </a>
</p>

<p align="center">
  For a clean and fast development environment
</p>


[![Build Status](https://travis-ci.org/Arbarwings/devpot.svg?branch=master)](https://travis-ci.org/Arbarwings/devpot)

## Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Installation](#installation)
- [On the ship](#Ontheship)

## Introduction
DevPot is a new version of [Maxlab Stacker](https://github.com/Maxlab/stacker). 

## Requirements
- Install [Docker](https://docs.docker.com/)
- Install [Docker Compose](https://docs.docker.com/compose/install/) > 1.20.0

## Installation

#### Get DevPot: 
```sh
$ git clone https://github.com/Arbarwings/devpot.git
# OR
$ git clone git@github.com:Arbarwings/devpot.git
```

#### Run in DevPot directory 
```sh 
# make a symbolic link to your folder with all your projects 
$ ln -s /your_path/to_all_your_own_projects ./workspace
# copy .env.dist to .env and change it
$ cp .env.dist .env
# Add docker group
$ sudo groupadd docker
# Add your user to the docker group
$ sudo usermod -aG docker $USER
# Now restart? Or find an other way to complete the usermod changes
$ docker-compose build && docker-compose up -d && docker-compose ps
$ mv ./test ./workspace
# Then open http://php.test.localhost/ in your browser
```

#### Run Docker as non root
- Create the docker group.
```sh
$ sudo groupadd docker
```
- Add your user to the docker group.
```sh
$ sudo usermod -aG docker $USER
```
- Log out and log back in so that your group membership is re-evaluated.

#### Move your projects
- Add your project in workspace folder `./workspace/<customer>/<projectname>` (no need to restart, this will work out of the box)
- Open http://project.customer.localhost/ in your browser (if you do not have dnsmasq, you have to add your hosts file manually)

## On the ship
- mailcatcher   -> schickling/mailcatcher:latest (all outgoing mail is sent to http://mail.localhost/)
- nginx         -> nginx:stable
- mysql         -> mysql:latest  
- php7xdebug    -> php:7.3 + xdebug
- dnsmasq  ->  dnsmasq:latest
- php7console   -> devpot console
- redis         -> redis:latest
- phpmyadmin    -> phpmyadmin:latest (http://phpmyadmin.localhost)

## Console
- *ZSH* + [oh-my-zsh](http://ohmyz.sh/)
- For backend: composer, php, phpunit, autocomplete
- For automation deploy: dep ([Deployer](http://deployer.org/))

## Create a Self-Signed Certificate
```sh
$ devpot create-certificate test # Replace test with your domain
```

## FAQ

#### Which settings in the configs for my projects?
- Database
    - You can access the database in your app config use `db` for mysql and `pgsql` for postgresql
        (files will be saved in the mysql directory so it will be saved after destroying or recreating the containers)
    ```yaml
      # Example for mysql
      parameters:
        database_host: mysql
        database_port: 3306
        database_name: sf
        database_user: root
        database_password: root
      
      # Example for redis
      parameters:
        database_host: redis
        database_port: 6379
    ```

#### What external ports are listening images?
- It's easy. For convenience, the external ports of the databases are offset by plus one. 
    For example, MySQL listens to port 3306 + 1 = 3307 and so on...
- Check the file [docker-compose.yml](/docker-compose.yml) for more 

#### Xdebug + PhpStorm configuration 
1. Go to Settings -> Languages & Frameworks -> PHP
2. Click the ... behind your interperter

#### How to contact the any instances DevPot in console?
You can do so:
```sh 
$ /your_path/to_devpot_folder/bin/devpot console
```
But, it will be much better:
```sh
# for bash
$ echo 'export PATH=/your_path/to_devpot_folder/bin:$PATH' >> ~/.bashrc && source ~/.bashrc 
# for ~/.zshrc
$ echo 'export PATH=/your_path/to_devpot_folder/bin:$PATH' >> ~/.zshrc && source ~/.zshrc
# then restart console and run
$ devpot console
```

#### Laravel5 completion
```sh
$ devpot console
$ cd to_laravel5_folder
$ la5 [tab*2]
```

## Commands
```sh
$ devpot usage # for list available commands
$ devpot console # for enter to console
$ devpot logs <cont_name> -f # for logs stream container
$ devpot build && devpot down && devpot up && devpot ps # for full rebuild
```
