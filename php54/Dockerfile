
FROM debian:wheezy

ENV DEBIAN_FRONTEND noninteractive

# Install base deps
RUN apt-get update && apt-get install -y --force-yes curl git mysql-client && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install php:
RUN apt-get update && apt-get install -y --force-yes apache2 php5 php5-curl php5-mysqlnd php5-gd php-apc php5-sqlite php5-mcrypt
RUN a2enmod rewrite && a2enmod php5

# Redirect Apache logging to stdout and stderr for capture using
# docker mechanisms.
RUN ln -sf /dev/stdout /var/log/apache2/access.log
RUN ln -sf /dev/stderr /var/log/apache2/error.log

# Install CLI tools:
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/bin/composer
RUN mkdir /usr/share/drush && cd /usr/share/drush && composer require drush/drush --prefer-dist && ln -s /usr/share/drush/vendor/bin/drush /usr/bin/drush && rm -r /root/.composer/cache

# Configure:
ADD apache-vhost.conf /etc/apache2/sites-available/default
ADD apache2-foreground /usr/local/bin/
RUN echo "apc.shm_size = 128M" >> /etc/php5/mods-available/apc.ini

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

WORKDIR /var/www
EXPOSE 80

CMD ["apache2-foreground"]
