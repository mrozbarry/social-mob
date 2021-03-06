# Social Mob

This application allows users to share and schedule their social mobs.

To create / join a social mob, the user must login. 

To avoid the registration step, this application uses oauth. 

## Local Setup

### Prerequisites

 - PHP 7.3
 - Composer
 - Node >= 12 LTS
 - Yarn
 - Mailtrap

### Setup

#### Initial configuration

```sh
# PHP Dependencies
composer install
# NPM Dependencies
yarn
# Setup env
cp .env.example .env
php artisan key:generate
```

#### Configuring the database

For ease of local development you can set the database to sqlite:

```diff
diff --git a/.env b/.env
index b0303cd..d7983fd 100644
--- a/.env.example
+++ b/.env.example
@@ -6,12 +6,7 @@ APP_URL=http://localhost

LOG_CHANNEL=stack

-DB_CONNECTION=mysql
-DB_HOST=127.0.0.1
-DB_PORT=3306
-DB_DATABASE=laravel
-DB_USERNAME=root
-DB_PASSWORD=
+DB_CONNECTION=sqlite

BROADCAST_DRIVER=log
CACHE_DRIVER=file
```

Once that is set up:

```sh
touch database/database.sqlite
php artisan migrate
php artisan db:seed
```

### Setup OAuth:

1. Go to github, and open your settings page
2. Open *Developer settings*
3. Create a new *OAuth App*
4. Set the homepage to `http://127.0.0.1:8000`
5. Set the callback url to `http://127.0.0.1:8000/oauth/callback`
6. Save it
7. Copy the *Client ID* to `./.env#GITHUB_CLIENT_ID` and *Client Secret* to `./.env#GITHUB_CLIENT_SECRET`

For more information, see:
 - [Laravel socialite](https://laravel.com/docs/7.x/socialite#configuration)

### Run the app

```sh
php artisan serve
```

The site should be available at [http://127.0.0.1:8000](http://127.0.0.1:8000).
