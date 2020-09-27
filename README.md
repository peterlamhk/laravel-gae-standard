# Laravel GAE Deployment Guide
A fresh installed Laravel **8.x** can be deployed on Google App Engine by following the below steps.

- [Minimal Installation](#minimal-installation)
- [Queues](#queues)
- [Task Scheduling](#task-scheduling)
- [Reference](#reference)


## Minimal Installation
1. Use the [Via Composer Create-Project](https://laravel.com/docs/8.x/installation#installing-laravel) method to get the Laravel.
```shell
composer create-project --prefer-dist laravel/laravel blog
```

2. Create ``app.yaml`` file as below:
```yaml
runtime: php73
entrypoint: serve public/index.php

env_variables:
  APP_KEY:
  LOG_CHANNEL: stderr
  SESSION_DRIVER: cookie
  VIEW_COMPILED_PATH: /tmp

instance_class: B1
manual_scaling:
  instances: 1
```

> ``runtime: php73`` can be changed to ``php74`` for PHP 7.4 (beta).
> 
> ``entrypoint: serve public/index.php`` is optional.
> 
> The error logs are written to ``stderr``, it is recommend to use [Cloud Logging](https://cloud.google.com/logging/docs/setup/php) by installing ``google/cloud-logging``.

3. Put a newly generated ``APP_KEY`` with command:
```shell
php artisan key:generate --show
```

4. Trust all proxies by setting ``$proxies`` in ``App\Http\Middleware\TrustProxies`` middleware for getting visitors' IPs.

5. Run ``gcloud app deploy`` to deploy as a new version.


## Queues
Instead of using database or redis as queue driver, we can use [Google Cloud Task](https://cloud.google.com/tasks) to collect jobs and execute them.

For the implentation, please refer to [retepmal/laravel-gae-queue-driver](https://github.com/retepmal/laravel-gae-queue-driver).


## Task Scheduling
There is a ``cron.yaml`` for sending scheduled requests to App Engine endpoints.

For the implentation, please refer to [retepmal/laravel-gae-task-scheduler](https://github.com/retepmal/laravel-gae-task-scheduler).


## Reference
- https://cloud.google.com/community/tutorials/run-laravel-on-appengine-standard
- https://github.com/GoogleCloudPlatform/php-docs-samples/issues/852
