# Installation

## Requirements
1. Access to terminal / SSH
2. Web server (Apache2, NGINX)
3. MySQL
4. NodeJS (for NPM)
5. PHP Extensions (enabled in php.ini)
    1. ext-dom
    2. ext-zip
    3. ext-gd
    4. ext-mysql
    5. All the other extensions needed by the Laravel framework.
6. Appropriate access to web folder.

Setup your web server (Apache/Nginx) to suit Laravel app. Make sure the `DocumentRoot` is set to the `public` folder.

## The Steps

There's **2** methods that you can use to install the system.

### Source Code (For Developers)
- Download the source code of this system from the GitHub repository.
- Copy `.env.example` file and rename to `.env`. Configure the `.env` file for database connection.
- Run `composer install` and `npm install` to install necessary dependencies.
    - You can also run `composer update` and `npm update` to update/upgrade necessary dependencies.
- Run `npm run prod` to and compile the assests.
- Generate the application key with `php artisan key:generate` command. This key will be stored in `.env` file and be used for encryption.
- (Optional) For production environment, set the `APP_DEBUG` value to `false` to prevent sensitive config to be exposed to the user.
- Upload the files to your web server. For installation in shared hosting, upload together with `vendor` folder. Head over to [Shared Hosting](#shared-hosting) section for further configurations.
- Go to your `/install` to proceed the installation. The script will add a new `admin` user and migrate the database.
- System installation is completed!

### Compiled Binary
- Download the the compiled binary from the [Downloads](#) page (coming soon).
- Configure the `.env` file for database connection.
- Generate the application key with `php artisan key:generate` command. This key will be stored in `.env` file and be used for encryption.
- (Optional) For production environment, set the `APP_DEBUG` value to `false` to prevent sensitive config to be exposed to the user.
- Upload the files to your web server.
- Go to your `/install` to proceed the installation. The script will add a new `admin` user and migrate the database.
- System installation is completed!

## Shared Hosting

Installation in shared hosting requires an additional setup. Most of the shared hosting out there does not provide full control of the server/hosting for instance no capability to install `supervisor (A process control system)`. Follow the steps below:

Open the `.env` file in editor and put the following code at the end of the file

```
SHARED_HOSTING_ENABLED=true
```

Seajell project require `queue:worker` need to be run, but shared hosting does not provide this feature out of the box. To accomplish put following line into `Cron Job` entry (Run this command for every minutes)

```
/<path-to-php> /<path-to-project>/artisan schedule:run >> /dev/null 2>&1

# Example /usr/local/bin/php /home/public/seajell/artisan schedule:run >> /dev/null 2>&1
```

Make sure to replace both `<path-to-php>` and `<path-to-project>` based on the php installation path and project path respectively in your shared hosting.

Make sure to upload `vendor` folder as well since there's no way to run composer install command unless your shared hosting does provide SSH access and any other permissions.

The root folder may be something like `html_www`, `htdocs`, `public_html`. So make sure to redirect all requests to the `public` folder. To do that you can add these lines to the `.htaccess` file (Apache 2):
``` apache
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteRule ^(.*)$ public/$1 [L]
</IfModule>
```
Some shared hosting provider does support Laravel app hosting. Contact your hosting provider for help. Unfortunately, this system won't work with free shared hosting due to limitations.
