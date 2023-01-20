# Docker, Angular, Laravel, Nginx, MySql, Redis demo project 

- NOTICE: This is a work in progress
  - At this point, I've got 
    - a new `Laravel` 9 app
    - and a new `Angular` 15.1.0
      - running in docker containers
        - which can be viewed 
          - in your browser 
            - on your local machine.



## TABLE OF CONTENTS

- Project Description
- Local Development Setup
  - Step 1: Docker Setup
  - Step 2: Laravel App Setup
  - Step 3: View in browser
  - Stop your containers
- Version Information
  - Angular
  - PHP, Laravel, and Composer
  - 2 Nginx Web Servers (1 serving Laravel & 1 serving Angular)
  - MySql
  - Redis







## Project Description

- This docker-compose project will make use of 
  - an `Angular frontend app` 
    - which will make requests to 
      - a `Laravel backend api app`.


- This project will make use of 3 Github repositories
  - The docker-compose yaml files --> `rnottinger/docker-compose-files`
  - The Angular app --> `rnottinger/client`
  - The Laravel app  --> `rnottinger/server`


- I used the techniques in the following article to put my `docker.compose.yml` files under git version control.
  - `https://www.codeconcisely.com/posts/how-to-put-docker-compose-files-under-git-version-control/`


## Local Development Setup

### Step 1: Docker Setup
- Install Docker Desktop on your local machine.
  - https://www.docker.com/products/docker-desktop/
- To create this docker-compose project: 
  - in your local development environment:
    - `cd ~/Documents/projects/docker`
      - First clone `rnottinger/docker-compose-files`
        - https://github.com/rnottinger/docker-compose-files
          - `docker-compose-files/`
            - `demo-project-compose.yml`
              - used during local development
            - `demo-project-compose-prod.yml`
              - used for production
            - `README.md`
              - the file you are reading right now!
            - `.gitignore`
              - list of files & folders that should not be under version control
    - Then create a new directory called `demo-app/`
      - `mkdir -p ~/Documents/projects/docker/demo-app`
      - `cd ~/Documents/projects/docker/demo-app`
        - clone `rnottinger/client`
        - clone `rnottinger/server`
    - Finally you will need to create 2 symbolic links (see article above for more detail)
      - `cd ~/Documents/projects/docker/docker-compose-files`
        - `ln -s $PWD/demo-project-compose.yml ~/Documents/projects/docker/demo-app/docker-compose.yml`
        - `ln -s $PWD/demo-project-compose-prod.yml ~/Documents/projects/docker/demo-app/docker-compose-prod.yml`
      - you should now have 2 symbolic links in `~/Documents/projects/docker/demo-app/` directory
    - Confirm you have the following directory structure:
      - `~/Documents/projects/docker/docker-compose-files/`
        - `~/Documents/projects/docker/docker-compose-files/demo-project-compose.yml`
        - `~/Documents/projects/docker/docker-compose-files/demo-project-compose-prod.yml`
        - `~/Documents/projects/docker/docker-compose-files/README.md`
        - `~/Documents/projects/docker/docker-compose-files/.gitignore`
    - Confirm you have the following directory structure:
      - `~/Documents/projects/docker/demo-app/`
        - `~/Documents/projects/docker/demo-app/client/<the-angular-app>`
        - `~/Documents/projects/docker/demo-app/server/<the-laravel-app>`
        - symbolic link --> `~/Documents/projects/docker/demo-app/docker-compose-prod.yml -> ~/Documents/projects/docker/docker-compose-files/demo-project-compose-prod.yml`
        - symbolic link --> `~/Documents/projects/docker/demo-app/docker-compose.yml -> ~/Documents/projects/docker/docker-compose-files/demo-project-compose.yml`
        - NOTE this directory is not under version control
          - `~/Documents/projects/docker/demo-app/`
            - We are only using this directory to spin up our containers using docker-compose commands in the terminal.
      - Open each of the following directories in a separate IDE window.
        - I'm using PHPStorm to open the `~/Documents/projects/docker/demo-app/server` Laravel project directory.
        - I'm using WebStorm to open the `~/Documents/projects/docker/demo-app/client` Angular project directory.
        - I'm using Webstorm to open the `~/Documents/projects/docker/docker-compose-files` docker-compose project directory.
    - Docker setup is now complete!
      - Start your containers!
        - `cd ~/Documents/projects/docker/demo-app`
          - you need to be in this docker-compose project directory 
            - so the following `docker-compose` commands
              - can find your `docker-compose.yml` file
                - when it is run.
        - `docker-compose up -d --build`
          - to build images
          - and start containers
          - and run containers in detached mode 
            - in other words, run in background 
              - so terminal can be used 
                - for other things
          - `NOTE`:
            - `docker-compose.yml` is `the default file` used
              - this is my local development docker-compose configuration file
            - otherwise use `the -f option` to specify a different filename
              - `docker-compose up -d --build -f docker-compose-prod.yml`
              - this is my production docker-compose configuration file
                - set more secure passwords
                - and use default/expected ports 
                - other settings that may differ in production 
                  - from local development docker-compose configuration

```shell


docker-compose ls
NAME                STATUS              CONFIG FILES
demo-app            running(5)          ~/Documents/projects/docker/demo-app/docker-compose.yml

 ~/Documents/projects/docker/demo-app  docker container ls --all \
  --filter label=com.docker.compose.project
CONTAINER ID   IMAGE                                  COMMAND                  CREATED       STATUS                   PORTS                                      NAMES
a6c593c1786c   nginx:stable-alpine                    "/docker-entrypoint.…"   2 hours ago   Up 2 hours               0.0.0.0:8088->80/tcp                       nginx-php
9b2a1add69b7   composer:latest                        "/docker-entrypoint.…"   2 hours ago   Exited (0) 2 hours ago                                              composer
0d1fcf50a7c7   demo-app-artisan                       "/var/www/html/artis…"   2 hours ago   Exited (0) 2 hours ago                                              artisan
942b8cda3b5f   rnottinger/angular-15.1.0              "/docker-entrypoint.…"   2 hours ago   Up 2 hours               0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   nginx-angular
b529b9c06387   rnottinger/php-8.1.14-laravel-9.48.0   "docker-php-entrypoi…"   2 hours ago   Up 2 hours               0.0.0.0:9000->9000/tcp                     php
15b76c97aef6   redis:7-alpine3.17                     "docker-entrypoint.s…"   2 hours ago   Up 2 hours               0.0.0.0:16379->6379/tcp                    redis
d92ee8dd9516   mysql:8                                "docker-entrypoint.s…"   2 hours ago   Up 2 hours               33060/tcp, 0.0.0.0:13306->3306/tcp         mysql
```

### Step 2: Laravel App Setup

- create `.env` file (REQUIRED)
  - by copying `.env.example`
- Generate `APP_KEY` in `.env` (REQUIRED)
  - `docker-compose run --rm artisan key:generate`
- Install composer dependencies (REQUIRED)
  - `docker-compose run --rm composer install`
    - the `vendor/` directory will be created
- Example: Install a single composer dependencies (OPTIONAL)
  - `docker-compose run --rm composer require --dev barryvdh/laravel-ide-helper`
- MySql Database Setup
  - confirm that the `.env` mysql environment settings match the `docker-compose.yml` mysql: service environment settings
    - `demo-app/server/.env`
    - `demo-app/docker-compose.yml`
- Run database migrations
  - `docker-compose run --rm artisan migrate`
- Example: Run any database seeders (OPTIONAL)
  - `docker-compose run --rm db:seed --class=PostSeeder`

### Step 3: View in browser

- Laravel app 
  - http://localhost:8088/
    - the nginx-php docker-compose service is listening on host port 8080
- Angular App
  - http://localhost/
    - the nginx-angular docker-compose service is listening on the default host port 80

### Stop your containers

- Make sure you are in your docker-compose project directory
  - `cd ~/Documents/projects/docker/demo-app`
  - `docker-compose down`






## Version Information

### Docker

```shell

docker version

Client:
 Cloud integration: v1.0.29
 Version:           20.10.21
 API version:       1.41
 Go version:        go1.18.7
 Git commit:        baeda1f
 Built:             Tue Oct 25 18:01:18 2022
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Desktop 4.15.0 (93002)
 Engine:
  Version:          20.10.21
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.18.7
  Git commit:       3056208
  Built:            Tue Oct 25 18:00:19 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.10
  GitCommit:        770bd0108c32f3fb5c73ae1264f7e503fe7b2661
 runc:
  Version:          1.1.4
  GitCommit:        v1.1.4-0-g5fd4c4d
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
  
  
  
  
docker-compose version
Docker Compose version v2.13.0

```

### Angular

- only the build assets generated from the `npm run build` which runs the `ng build` command
  - are copied to the `rnottinger/angular-15.1.0` image.
    - This is the only the pieces we need
      - at application run-time
        - and we leave all of the build stuff (node_modules/, code, etc)
    - ..basically all the stuff needed for local development
      - are not needed during application run time.
- ..therefore you will need to install `node`, `npm`, `angular-cli` on your local machine
  - I recommend installing `nvm` for managing node & npm versions.
    - to switch between `node` versions
    - or to install different node version


```shell

  
~/Documents/projects/docker/demo-app  docker-compose exec nginx-angular sh

/ # cd /usr/share/nginx/html/
/usr/share/nginx/html # ls
3rdpartylicenses.txt           favicon.ico                    main.100698de03b18aa5.js       runtime.7ae29a296d479790.js
50x.html                       index.html                     polyfills.be126616119fee81.js  styles.ef46db3751d8e999.css





 ~/Documents/projects/docker/demo-app  nvm list
        v8.17.0
       v12.20.0
       v16.10.0
->     v16.13.2
        v17.4.0
       v18.10.0
       v18.12.1
default -> 12.20.0 (-> v12.20.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v18.12.1) (default)
stable -> 18.12 (-> v18.12.1) (default)
lts/* -> lts/hydrogen (-> v18.12.1)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.2 (-> N/A)
lts/gallium -> v16.19.0 (-> N/A)
lts/hydrogen -> v18.12.1



 richardottinger@richard-mb  ~/Documents/projects/docker/demo-app  nvm use 18.10.0
Now using node v18.10.0 (npm v8.19.2)



 richardottinger@richard-mb  ~/Documents/projects/docker/demo-app  ng version

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/


Angular CLI: 15.1.0
Node: 18.10.0
Package Manager: npm 8.19.2
OS: darwin x64

Angular:
...

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.1501.0 (cli-only)
@angular-devkit/core         15.1.0 (cli-only)
@angular-devkit/schematics   15.1.0 (cli-only)
@schematics/angular          15.1.0 (cli-only)




 ~/Documents/projects/docker/demo-app  node -v
v18.10.0


  ~/Documents/projects/docker/demo-app  npm -v
8.19.2


```

### PHP, Laravel, and Composer

```shell

 ~/Documents/projects/docker/demo-app  docker-compose exec php bash
 
ce4c3ac44855:/var/www/html# php -v
PHP 8.1.14 (cli) (built: Jan  9 2023 19:08:33) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.14, Copyright (c) Zend Technologies



ce4c3ac44855:/var/www/html# php --ini
Configuration File (php.ini) Path: /usr/local/etc/php
Loaded Configuration File:         (none)
Scan for additional .ini files in: /usr/local/etc/php/conf.d
Additional .ini files parsed:      /usr/local/etc/php/conf.d/docker-fpm.ini,
/usr/local/etc/php/conf.d/docker-php-ext-bcmath.ini,
/usr/local/etc/php/conf.d/docker-php-ext-pdo_mysql.ini,
/usr/local/etc/php/conf.d/docker-php-ext-redis.ini,
/usr/local/etc/php/conf.d/docker-php-ext-sodium.ini


 
 
a2812d9ea332:/var/www/html# php artisan --version
Laravel Framework 9.48.0


# or you can run the artisan container
 ~/Documents/projects/docker/demo-app  docker-compose run --rm artisan --version
[+] Running 1/0
 ⠿ Container mysql  Running                                                                                                                                                            0.0s
Laravel Framework 9.48.0


# Run the composer container
 ~/Documents/projects/docker/demo-app  docker-compose run --rm composer
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.5.1 2022-12-22 15:33:54

# ...



```

### 2 Nginx Web Servers (1 serving Laravel, 1 serving Angular)

```shell

# the nginx web server listening on port 8088 for the laravel app
~/Documents/projects/docker/demo-app  docker-compose exec nginx-php /bin/sh

/ # nginx -v
nginx version: nginx/1.22.1

/ # nginx -V
nginx version: nginx/1.22.1
built by gcc 11.2.1 20220219 (Alpine 11.2.1_git20220219)
built with OpenSSL 1.1.1o  3 May 2022 (running with OpenSSL 1.1.1s  1 Nov 2022)
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-perl_modules_path=/usr/lib/perl5/vendor_perl --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-Os -fomit-frame-pointer -g' --with-ld-opt=-Wl,--as-needed,-O1,--sort-common






# the nginx web server listing on port 80 for the angular app
 ~/Documents/projects/docker/demo-app  docker-compose exec nginx-angular sh
/ # nginx -v

nginx version: nginx/1.22.1
/ # nginx -V
nginx version: nginx/1.22.1
built by gcc 11.2.1 20220219 (Alpine 11.2.1_git20220219)
built with OpenSSL 1.1.1o  3 May 2022 (running with OpenSSL 1.1.1s  1 Nov 2022)
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-perl_modules_path=/usr/lib/perl5/vendor_perl --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-Os -fomit-frame-pointer -g' --with-ld-opt=-Wl,--as-needed,-O1,--sort-common


```

### MySql


```shell

sh-4.4# mysql -V
mysql  Ver 8.0.32 for Linux on x86_64 (MySQL Community Server - GPL)


sh-4.4# mysqld --help
mysqld  Ver 8.0.32 for Linux on x86_64 (MySQL Community Server - GPL)





# since I have the mysql client installed on my local machine I can..
~/Documents/projects/docker/demo-app  mysql -u root --host=127.0.0.1 -P13306 -proot

mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.32 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| homestead          |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> use homestead;
Database changed
mysql> show tables;
Empty set (0.00 sec)



mysql> show variables like "%version%";
+--------------------------+------------------------------+
| Variable_name            | Value                        |
+--------------------------+------------------------------+
| admin_tls_version        | TLSv1.2,TLSv1.3              |
| immediate_server_version | 999999                       |
| innodb_version           | 8.0.32                       |
| original_server_version  | 999999                       |
| protocol_version         | 10                           |
| replica_type_conversions |                              |
| slave_type_conversions   |                              |
| tls_version              | TLSv1.2,TLSv1.3              |
| version                  | 8.0.32                       |
| version_comment          | MySQL Community Server - GPL |
| version_compile_machine  | x86_64                       |
| version_compile_os       | Linux                        |
| version_compile_zlib     | 1.2.13                       |
+--------------------------+------------------------------+
13 rows in set (0.01 sec)

```

### Redis

```shell

 ~/Documents/projects/docker/demo-app  docker-compose exec redis sh
 
/data # redis- <hit tab>
redis-benchmark  redis-check-aof  redis-check-rdb  redis-cli        redis-sentinel   redis-server

/data # redis-cli -v
redis-cli 7.0.8

/data # redis-server -v
Redis server v=7.0.8 sha=00000000:0 malloc=jemalloc-5.2.1 bits=64 build=69f3c0819f3dfca5
/data #

```
