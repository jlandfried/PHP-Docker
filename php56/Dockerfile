
FROM debian:jessie

# Install base deps:
RUN apt-get update && apt-get install -y --force-yes curl git mysql-client && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install apache
RUN apt-get update && apt-get install -y --force-yes apache2 php5 php5-curl php5-mysqlnd php5-gd && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN a2enmod rewrite && a2enmod php5

# Redirect Apache loggin to stdout and stderr for capture using
# docker mechanisms.
RUN ln -sf /dev/stdout /var/log/apache2/access.log
RUN ln -sf /dev/stderr /var/log/apache2/error.log

# Install CLI tools:
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/bin/composer
RUN mkdir /usr/share/drush && cd /usr/share/drush && composer require drush/drush && ln -s /usr/share/drush/vendor/bin/drush /usr/bin/drush

# Configure:
RUN sed -i "/date\.timezone =/c\date.timezone = America\/New_York" /etc/php5/cli/php.ini /etc/php5/apache2/php.ini
ADD apache-vhost.conf /etc/apache2/sites-available/000-default.conf
ADD apache2-foreground /usr/local/bin/
RUN echo "opcache.memory_consumption = 128" >> /etc/php5/mods-available/opcache.ini

WORKDIR /srv
EXPOSE 80

CMD ["apache2-foreground"]
