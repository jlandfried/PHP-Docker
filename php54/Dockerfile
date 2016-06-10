
FROM debian:wheezy

# common-php54
ENV DEBIAN_FRONTEND noninteractive
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

# Add backports for git > 1.7
RUN echo "deb http://http.debian.net/debian wheezy-backports main" >> /etc/apt/sources.list \
  # Install base deps
  && apt-get update \
  && apt-get install -y -t wheezy-backports git \
  && apt-get install -y --force-yes bzip2 curl mysql-client \
  # Install php:
  && apt-get install -y --force-yes apache2 php5 php5-curl php5-mysqlnd php5-gd php-apc php5-sqlite php5-mcrypt \
  && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Configure Apache:
RUN a2enmod rewrite php5 \
  # Redirect Apache logging to stdout and stderr for capture using
  # docker mechanisms.
  && ln -sf /dev/stdout /var/log/apache2/access.log \
  && ln -sf /dev/stderr /var/log/apache2/error.log \
  && echo "apc.shm_size = 128M" >> /etc/php5/mods-available/apc.ini


# Install CLI tools:
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/bin/composer \
  && mkdir /usr/share/composer \
  && cd /usr/share/composer \
  && composer init --no-interaction && composer config bin-dir /usr/local/bin \
  && composer require drush/drush --prefer-dist \
  && rm -r /root/.composer/cache

# Configure:
ADD apache-vhost.conf /etc/apache2/sites-available/default
ADD apache2-foreground /usr/local/bin/

WORKDIR /var/www
EXPOSE 80

CMD ["apache2-foreground"]
# /common-php54

COPY test.sh /test.sh
