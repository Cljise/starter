## PROJET

Projet (ex : Tool - API powered by Laravel 10.16.1) 

## Server Requirements (default)

-   `PHP FPM 8.2.1`
-   `Apache 2.0`
-   `MariaDB 10.4`

PHP Extensions:

-   `OpenSSL`
-   `Mbstring`
-   `Tokenizer`
-   `pdo_mysql`
-   `pdo_sqlite`
-   `SimpleXML`

## Setup and run

On a first install, scaffold and run the entire project by running:

```shell
> ./bin/init
```

Smile and start coding 🖖

## Usage

### Development

The project is using Docker in development.

Build images and start services:

```shell
> ./bin/up
```

-   Application will be available at http://localhost
-   phpMyAdmin will be available at http://localhost:8080

> Note: the page might take a while to load the first time due to the cache folder generation that is going to be created and the fact that its link as a Docker volume

Stops services:

```shell
> ./bin/down
```

#### Deployment

Based on what you'd like to deploy, run the following command:

```shell
> ./bin/deploy-[staging|production]
```

On a first install run:

```
> ./bin/deploy-[staging|production] --first-install
```

> This will initiate the project keys and auth secrets

## Helper scripts

Following scripts are available in the `bin` folder.

### artisan


```shell
> ./bin/artisan --version
php artisan --version

Laravel Framework 10.16.1
```

### composer


```shell
> ./bin/composer --version
composer --version

Composer version 2.5.8
```

### container

Running the following command takes you inside the `app` container under user uid(1000) (same with host user)

```shell
> ./bin/container
devuser@8cf37a093502:/var/www/html$
```

### db

Running the following command connects to your database container's daemon using mysql client.

```shell
> ./bin/db
mysql>
```