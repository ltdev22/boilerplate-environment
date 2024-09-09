# boilerplate-environment
This is a template repository to use for creating new projects/repositories with the same directory structure, branches, files etc


## Instructions

Run `cp webapp.env.example webapp.env` to create a new _.env_ file and setup the variables.

To run create a php project, like a fresh laravel installation run `docker run --rm --interactive --tty --volume $PWD:/app composer create-project laravel/laravel applications`
This will create a fresh laravel app inside the _applications_ directory.

To run any composer commands you can use the _composer_ service by running `docker compose run --rm composer <command>`

To run any artisan commands you can use the _artisan_ service by running `docker compose run --rm artisan <command>`

## Database

If there app in the _applications_ folder is a Laravel app make sure to specify in the docker-compose.yml the _.env_ with the *exact* order
```
 env_file:
    - "applications/.env" // Laravel .env file
    - "webapp.env"
```
This way will make sure on `docker compose up` that the Laravel environment variables will load first!

#### Setting up database connection and creating a new user
1. Run `docker-compose exec mysql /bin/bash`
2. Within the container run `mysql -u root -p` and provide the password for root user (root)
3. To create a new user and grant priviledges run the following query. 
    Make sure you replace `MYSQL_DATABASE` with the database name, `MYSQL_USER` with the db user and `MYSQL_PASSWORD` with the db user's password.
    These are usually defined in the Laravel .env file if running a Laravel app.
    ```
    grant all privileges on `MYSQL_DATABASE`.* to 'MYSQL_USER'@'%' identified by 'MYSQL_PASSWORD';
    ```
4. Confirm the user has been created and also has the right privileges
    ```
    show grants for 'MYSQL_USER'@'%';
    ```

#### Connecting to MySQL Gui (TablePlus, Sequel Pro, Mysql Workbench etc)
Replace the env variables with the values
1. Host `localhost` or `127.0.0.1`
2. User `MYSQL_USER`
3. Password `MYSQL_PASSWORD`
4. Database `MYSQL_DATABASE`
5. Port `3306`