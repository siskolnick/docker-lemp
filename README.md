DOCKER-LEMP
========================

This repository contains image composer for PHP + Nginx + MySQL , its purpose is to work with PHP projects current version PHP 7.4

Requirement
-------------
- Docker 

Token
-------
The ENV contains variables for the MySQL image settings (root,user,pwd) 

- [DATABASE_NAME]: Original test db
- [DATABASE_USER] : Database User
- [DATABASE_PASSWORD] : Database Password
- [DATABASE_ROOT_PASSWORD] : Database Root password

Installation
--------------

Here is some steps you need to perform.

- Clone it from GIT
- Inside /app folder clone your app project(s)

- From your host machine folder root (CMD), build and start the containers with
```sh
docker-compose up -d --build 
```

```
- Copy the php.ini for PHP, this is also done inside the CLI of PHP container
```sh
/usr/local/etc/php cp php.ini-development php.ini
```



App projects and Nginx
-------
Inside "/docker/nginx/sites/default.conf" you'll find server definition for each of your projects, right now you need to specify a different port for each one. Whithin the cointainer you'll find it in "/var/www/"

