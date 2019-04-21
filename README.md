## Description
The scope of this project is to install the necessary Docker services so to be able to use [PHP](php.net), [PHPUnit](https://phpunit.de/) testing framework, [Xdebug](https://xdebug.org/). In addition it will be added a tutorial to explain how to launch it from [PHPStorm](https://www.jetbrains.com/phpstorm/).

# Installation
```
$ cd /home/aurelien/my-projects/
```

Clone the repository:

```
$ /usr/bin/git clone git@github.org:aurelienlair/php-docker-services.git 
$ cd php-docker-services
``` 

Add your Github [token](https://github.com/settings/tokens) into the .env file

Change the volume's path of your PHP project on the docker-compose.yml instead of `/home/aurelien/my-projects/my-project-source`

Build the containers:

```
$ docker-compose build 
``` 

Run the containers:

```
$ docker-up -d 
``` 
