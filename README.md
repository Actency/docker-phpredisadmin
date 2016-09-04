# About this repo

This is the Actency Docker image for phpredisadmin.

# Example usage with docker-compose

    version: '2'
    services:
      # web with xdebug - actency images
      web:
        # actency/docker-apache-php available tags: latest, 7.0, 5.6, 5.5, 5.4, 5.3
        image: actency/docker-apache-php:7.0
        ports:
          - "80:80"
          - "9000:9000"
        environment:
          - SERVERNAME=example.loc
          - SERVERALIAS=example2.local *.example2.local
          - DOCUMENTROOT=web
        volumes:
          - /home/docker/projets/projet1/:/var/www/html/
        links:
          - database:mysql
          - mailhog
          - solr
          - redis
        tty: true

      # database container - actency images
      database:
        # actency/docker-mysql available tags: latest, 5.7, 5.6, 5.5
        image: actency/docker-mysql:5.7
        ports:
          - "3306:3306"
        environment:
          - MYSQL_ROOT_PASSWORD=mysqlroot
          - MYSQL_DATABASE=example
          - MYSQL_USER=example_user
          - MYSQL_PASSWORD=mysqlpwd

      # phpmyadmin container - actency images
      phpmyadmin:
        image: actency/docker-phpmyadmin
        ports:
          - "8010:80"
        environment:
          - MYSQL_ROOT_PASSWORD=mysqlroot
          - UPLOAD_SIZE=1G
        links:
          - database:mysql

      # mailhog container - official images
      mailhog:
        image: mailhog/mailhog
        ports:
          - "1025:1025"
          - "8025:8025"

      # solr container - actency images
      solr:
        # actency/docker-solr available tags: latest, 6.1, 6.0, 5.5, 5.4, 5.3, 5.2, 5.1, 5.0, 4.10, 3.6
        image: actency/docker-solr:6.1
        ports:
          - "8080:8983"

      # redis container - official images
      redis:
        image: redis:latest
        ports:
          - "6379"

      # phpRedisAdmin container - actency images
      phpredisadmin:
        image: actency/docker-phpredisadmin
        ports:
          - "9900:80"
        environment:
          - REDIS_1_HOST=redis
        links:
          - redis

See the [Docker Hub page](https://hub.docker.com/r/actency/docker-phpredisadmin/) for the full readme on how to use this and for other information.
