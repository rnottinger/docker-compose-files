# Docker, Angular, Laravel, Nginx, MySql, Redis demo project 

- NOTICE: This is a work in progress
  - At this point, I've got 
    - a new `Laravel` 9 app
    - and a new `Angular` 15.1.0
      - running in docker containers
        - which can be viewed 
          - in your browser 
            - on your local machine.



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
        - `docker-compose-files/`
          - `demo-project-compose.yml`
            - used during local development
          - `demo-project-compose-prod.yml`
            - used for production
          - `README.md`
            - the file you are reading right now!
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
    - Confirm you have the following directory structure:
      - `~/Documents/projects/docker/demo-app/`
        - `~/Documents/projects/docker/demo-app/client/<the-angular-app>`
        - `~/Documents/projects/docker/demo-app/server/<the-laravel-app>`
        - symbolic link --> `~/Documents/projects/docker/demo-app/docker-compose-prod.yml -> ~/Documents/projects/docker/docker-compose-files/demo-project-compose-prod.yml`
        - symbolic link --> `~/Documents/projects/docker/demo-app/docker-compose.yml -> ~/Documents/projects/docker/docker-compose-files/demo-project-compose.yml`
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
          - `NOTE`:
            - `docker-compose.yml` is `the default file` used
            - otherwise use `the -f option` to specify a different filename
              - `docker-compose up -d --build -f docker-compose-prod.yml`
        - `docker-compose container ls`
          - to determine if containers are started.


  
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


### View in browser

- Laravel app 
  - http://localhost:8088/
- Angular App
  - http://localhost/


### Stop your containers

- Make sure you are in your docker-compose project directory
  - `cd ~/Documents/projects/docker/demo-app`
  - `docker-compose down`
