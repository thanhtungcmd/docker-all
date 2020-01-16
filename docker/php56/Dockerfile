FROM php:5.6-fpm

# Install package
RUN apt-get update
RUN apt-get install vim git zip unzip cron libpng-dev libcurl3-dev libxml2-dev -y
RUN apt-get install gcc autoconf libssl-dev build-essential -y

# Database
#RUN pecl install mongodb \
#	&& docker-php-ext-enable mongodb
RUN pecl install mongo \
    && docker-php-ext-enable mongo
RUN docker-php-ext-install gd zip curl pdo_mysql soap

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE "9000"