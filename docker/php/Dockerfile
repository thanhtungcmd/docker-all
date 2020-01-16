FROM php:7.2-fpm

# Install package
RUN apt-get update
RUN apt-get install vim git zip unzip libpng-dev libzip-dev librabbitmq-dev libicu-dev cron -y

# Database
RUN pecl install mongodb \
	&& docker-php-ext-enable mongodb
RUN docker-php-ext-install pdo_mysql zip bcmath gd
RUN pecl install amqp \
    && docker-php-ext-enable amqp

# Imap
RUN apt-get install libkrb5-dev libc-client-dev -y
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-install imap

# Intl
RUN apt-get install zlib1g-dev libicu-dev -y
RUN docker-php-ext-configure intl
RUN docker-php-ext-install intl

# Timezone
RUN echo "date.timezone = Asia/Ho_Chi_Minh" > /usr/local/etc/php/php.ini
RUN echo "upload_max_filesize = 1024M" >> /usr/local/etc/php/php.ini
RUN echo "post_max_size = 1024M" >> /usr/local/etc/php/php.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE "9000"