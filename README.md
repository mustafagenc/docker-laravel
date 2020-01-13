# docker-laravel

Build Laravel's development environment using docker.
PHP7.4/MySQL8.0/nginx/redis/node

## Build for Windows

## Git settings

```
$ git config --global core.autocrlf false
```

## A. Create Laravel Project

Build a new Laravel project.

```
$ git clone git@github.com:ucan-lab/docker-laravel.git
$ cd docker-laravel
$ cp .env-example .env
$ docker-compose up -d --build
$ docker-compose exec app composer create-project --prefer-dist "laravel/laravel=5.8.*" .
$ docker-compose exec app composer require predis/predis
```

http://127.0.0.1:10080

## B. Clone and Install

It is assumed that Laravel is already installed.

```
$ git clone git@github.com:ucan-lab/docker-laravel.git
$ cd docker-laravel
```

### If you change the project directory

docker `.env` file

```
PROJECT_PATH=./src
```

### Laravel Install

```
$ docker-compose up -d --build
$ docker-compose exec app composer install
$ docker-compose exec app cp .env.example .env
$ docker-compose exec app php artisan key:generate
$ docker-compose exec app php artisan migrate:fresh
$ docker-compose exec app php artisan make:auth
```

http://127.0.0.1:10080

## Tips

### Running Migrations

```
$ docker-compose exec app php artisan migrate
```

### Running Testings

```
$ docker-compose exec app ash -l
$ cp .env.example .env.testing
$ php artisan key:generate --env testing
$ sed -i -e 's/<php>/<php>\n        <env name="DB_HOST" value="db-testing" force="true"\/>/' phpunit.xml
$ ./vendor/bin/phpunit
```

### Send Test Mail

```
$ docker-compose exec app php artisan tinker
Mail::raw('test mail',function($message){$message->to('test@example.com')->subject('test');});
```

http://127.0.0.1:18025

### Composer dump autoload

```
$ docker-compose exec app composer dump-autoload
```

### MySQL connection

```
$ docker-compose exec db bash -c 'mysql -u $$MYSQL_USER -p$$MYSQL_PASSWORD $$MYSQL_DATABASE'
```

### Node(npm)

```
$ docker-compose run node npm install
$ docker-compose run node npm run dev
```

### Node(yarn)

```
$ docker-compose run node yarn install
$ docker-compose run node yarn run dev
```

### Redis for Laravel

```
$ docker-compose exec app php artisan tinker
use Illuminate\Support\Facades\Redis;
Redis::set('name', 'hoge');
Redis::get('name');
```

### Redis cli

```
$ docker-compose exec redis redis-cli
```

### Clear database volume

```
$ docker-compose down --volumes --rmi all
$ docker-compose up -d --build
```


## [Build for Mac](https://github.com/ucan-lab/docker-laravel/wiki/Build-for-Mac)