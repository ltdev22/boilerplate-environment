# boilerplate-environment
This is a template repository to use for creating new projects/repositories with the same directory structure, branches, files etc


## Instructions

Run `cp webapp.env.example webapp.env` to create a new _.env_ file and setup the variables.

### Creating a fresh application
To create a php project, like a fresh laravel, run `docker run --rm --interactive --tty --volume $PWD:/app composer create-project laravel/laravel working-directory`
This will create a fresh laravel app inside the _working-directory_ directory.

### Configuring the app
To set up a local host, like  `http://boilerplate.local`, need to edit the _/etc/hosts/_ and add `127.0.0.1 http://boilerplate.local`. Then server config file set the server name to the host.
To enable https locally with mkcert, run through the terminal `mkcert boilerplate` inside _docker/ssl_ folder. This will create the to pem files.

For Laravel applications to maintain a single `.env` file, copy all the variables from _.env_ to _docker/webapp.env_. Then in the `bootstrap/app.php` file, right before the last line where `return $app;` is,
add the following two lines of code and restart docker.
```
$app->useEnvironmentPath(dirname(__DIR__, 2) . '/docker');
$app->loadEnvironmentFrom('webapp.env');
```

### Running commands through the terminal

To run easily various commands through the terminal, you can use the _develop_ executable. 
Run `./develop -h` or `./develop --help` to see how it works. If you don't want to use _develop_ 
(although it is the recommended way) you can run the commands directly using _docker compose_ like this example: 
```
docker compose run --rm composer require laravel/breeze:"^1.26"
docker compose run --rm artisan breeze:install
docker compose run --rm artisan migrate
docker compose run --rm npm install
docker compose run --rm npm run build
```

### Database

If the app in the _working-directory_ folder is a Laravel app make sure to specify in the docker-compose.yml the _.env_ with the *exact* order
```
 env_file:
    - "./docker/webapp.env"
    - "working-directory/.env"
```
This way will make sure that all environment variables will load first and then the ones that Laravel require on `docker compose up`, see for example
`DB_DATABASE=${MYSQL_DATABASE}`.
**TODO** This is temporary - need to figure out how to use only one .env file and where is best to place it.


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
1. Host is usually either `localhost` or `127.0.0.1` the rest are same to the env variables
2. User `MYSQL_USER`
3. Password `MYSQL_PASSWORD`
4. Database `MYSQL_DATABASE`
5. Port `MYSQL_PORT`