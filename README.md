# docker-lempp
This is a repo of my docker dev setup containing postgresql, mysql, nginx and php containers. It's obviously aimed for development and not for production.


## detail
* `.env`file contains username and passwords for databases
* `php-devmach` docker image extends `php:7-fpm-alpine3.7` by adding some additional modules
* **php** and **nginx** runs in different cotainers but share same volume.