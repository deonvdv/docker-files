FROM php:5.6.28-cli

MAINTAINER Deon van der Vyver <deon@creativelogic.es>

RUN apt-get update && apt-get -y install zip unzip git curl php5-mcrypt php5-json php5-mysqlnd php5-sqlite && apt-get -y autoremove && apt-get clean && rm -rf /var/lib/apt/lists/*
RUN docker-php-ext-install -j$(nproc) \
    bcmath \
    mbstring

# Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"
ONBUILD RUN composer self-update

# Link the code
#RUN /bin/mkdir /var/www
COPY my-app /var/www/my-app

WORKDIR /var/www/my-app

# Composer Install
RUN composer install

#CMD ["php /var/www/my-app/artisan","queue:work","--sleep=3","--tries=3","--timeout=30"]
CMD php /var/www/my-app/artisan queue:work --sleep=3 --tries=3 --timeout=30
