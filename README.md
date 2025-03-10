# Deploy

<p align="center">
    <a href="https://travis-ci.org/itsjeffro/deploy"><img src="https://travis-ci.org/itsjeffro/deploy.svg?branch=master" alt="Build Status"></a>
    <a href="https://packagist.org/packages/itsjeffro/deploy"><img src="https://poser.pugx.org/itsjeffro/deploy/license.svg"></a>
    <a href="https://packagist.org/packages/itsjeffro/deploy"><img src="https://poser.pugx.org/itsjeffro/deploy/d/total.svg" alt="Total Downloads"></a>
</p>

## Introduction

Deploy provides a dasboard for existing Laravel applications to manage zero-downtime deployments.

<p align="center">
    <img src="https://res.cloudinary.com/dz4tjswiv/image/upload/v1568524157/dashboard.png">
</p>

## Server Requirements

* openssh-clients (To establish ssh connections)

## Installation

Prior to installing this package, it is assumed you have already configured an auth gaurd with the App\User model for your Laravel application. 

Using composer, install the package into your Laravel project:
```
composer require itsjeffro/deploy
```

Next, you will need to register the package's service provider class under the providers array 
in the config/app.php configuration file.

```
Deploy\DeployServiceProvider::class
```

Publish the package's config and assets:

```
php artisan vendor:publish --tag=deploy-config
php artisan vendor:publish --tag=deploy-assets
```

When you update to a new release of deploy package, you should make sure you update the assets using the command below.

```
php artisan vendor:publish --tag=deploy-assets --force
```

Run the package's migrations:

```
php artisan migrate
```

### Configuration

After publishing the assets, the primary config file will be located at `config/deploy.php`. 

Before using the deploy application, you will need to set up your repository provider and a proper queue driver.

### Available Providers

Update your `.env` file to include a provider of your choice. A minimum of one provider is required.

__Bitbucket__

https://confluence.atlassian.com/bitbucket/oauth-on-bitbucket-cloud-238027431.html

```
BITBUCKET_OAUTH_KEY=client_id
BITBUCKET_OAUTH_SECRET=client_secret
```

__Github__

https://developer.github.com/apps/building-oauth-apps/creating-an-oauth-app/

```
GITHUB_OAUTH_KEY=client_id
GITHUB_OAUTH_SECRET=client_secret
```

### Queue connection

In your `.env` file, `QUEUE_CONNECTION` should be changed to another connection other than "sync". This way the process workers can correctly work on the server hosting the deployment package. 

With the connection updated, paths such as  `/home/user/.ssh/known_hosts` will be used instead of `/var/www/.ssh/known_hosts`.

Worker roles also handle:

* Creating individual SSH private keys for each project`s server

* Handling deployments

* Managing the projects .env file

* Testing server connections

## Broadcasting

To allow real-time feedback when a deployment or server connection has started or finished, you may set up the application 
to utlise Laravel's broadcasting feature.

### Publish Resources

TBA

### Publish Views

TBA

### Configuration

TBA

Note: You may need to restart the queue worker to pick up on your configuration updates.

## Deployments

Given that the deployment process uses symlinks. The user performing the deployment actions will be required to have the ability to reload the php-fpm service on the server. You may create a deployment hook which does this after the "Clean Up" action.

## Further reading

* [Projects](docs/project.md)
* [Deployments](docs/deployment.md)
* [Deployment hooks](docs/deployment-hooks.md)
* [Environment](docs/environment.md)
* [Linked folders](docs/linked-folders.md)
* [Folder structure](docs/folder-structure.md)
