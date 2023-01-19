# Docker, Angular, Laravel, Nginx, MySql, Redis demo project 

- NOTICE: This is a work in progress
  - At this point, I've got 
    - a new Laravel 9 app
    - and a new Angular 15.1.0
    - running in docker containers.



## Project Description

- This docker-compose project will make use of 
  - an `Angular frontend app` 
    - which will make requests to 
      - a `Laravel backend api app`.


- This project will make use of 3 Github repositories
  - The docker-compose yaml files --> `rnottinger/docker-compose-files`
  - The Angular app --> `rnottinger/client`
  - The Laravel app  --> `rnottinger/server`


- I used the techniques in the following article to put my `docker.compose.yml` files under version control.
  - `https://www.codeconcisely.com/posts/how-to-put-docker-compose-files-under-git-version-control/`


## Local Development Setup

### Step 1: Docker Setup
- Install Docker Desktop on your local machine (I'm using a mac. Good luck if your on windows.)
- To create this docker-compose project in your local development environment
  - `cd ~/Documents/projects`
    - First clone `rnottinger/docker-compose-files`
      - `docker-compose-files/`
        - `demo-project-compose.yml`
          - used during local development
        - `demo-project-compose-prod.yml`
          - used for production
        - `README.md`
          - the file you are reading right now!
  - Then create a new directory called `demo-app/`
    - `mkdir -p ~/Documents/projects/demo-app`
    - `cd ~/Documents/projects/demo-app`
      - clone `rnottinger/client`
      - clone `rnottinger/server`
  - Finally you will need to create 2 symbolic links (see article above for more detail)
    - `cd ~/Documents/projects/docker-compose-files`
      - `ln -s $PWD/demo-project-compose.yml ~/Documents/projects/docker/demo-app/docker-compose.yml`
      - `ln -s $PWD/demo-project-compose-prod.yml ~/Documents/projects/docker/demo-app/docker-compose-prod.yml`
    - you should now have 2 symbolic links in `~/Documents/projects/docker/demo-app/` directory
  - Docker setup is now complete!
  - `cd ~/Documents/projects/docker/demo-app`
  - `docker-compose up -d --build`
    - to build images and start containers
  - `docker-compose down`
    - to stop containers


### Step 2: Laravel App Setup

- Containers should be started up
  - `docker-compose container ls`
- Install composer dependencies
  - `docker-compose run --rm composer install`
    - the `vendor/` directory will be created
- Example: Install a single composer dependencies (OPTIONAL)
  - `docker-compose run --rm composer require --dev barryvdh/laravel-ide-helper`
- Run database migrations
  - `docker-compose run --rm artisan migrate`
- Example: Run any database seeders (OPTIONAL)
  - `docker-compose run --rm db:seed --class=PostSeeder`


### View in browser

- Laravel app 
  - http://localhost:8088/
- Angular App
  - http://localhost/
