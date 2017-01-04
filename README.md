Last Call Media PHP Dockerfiles
===============================

These Docker files are for building various PHP related docker images for use with Drupal.

They all offer the following tools:

* PHP (with MySQL support, sqlite, GD, curl, mcrypt and an OpCode cache)
* Apache, configured to serve /var/www with an exposed port on 80
* Composer
* Drush

Environment Variables
---------------------
APACHE_RUN_USER (www-data): The user to run Apache as.
APACHE_RUN_GROUP (www-data): The group to run Apache as.
APACHE_REQUEST_WORKERS (150): The MaxRequestWorkers setting.
TZ (none): The Timezone to use for PHP.
