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

# Soap
#RUN apt-get update -y
RUN rm /etc/apt/preferences.d/no-debian-php
RUN apt-get install libxml2-dev php-soap -y
RUN docker-php-ext-install soap

# Excel
RUN apt-get install zlib1g-dev libpng-dev -y
RUN docker-php-ext-install zip
RUN docker-php-ext-install gd
RUN docker-php-ext-install xml

# ORACLE oci
RUN apt-get update && apt-get install -qqy git unzip libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libaio1 wget && apt-get clean autoclean && apt-get autoremove --yes &&  rm -rf /var/lib/{apt,dpkg,cache,log}/

RUN mkdir /opt/oracle \
    && cd /opt/oracle

ADD instantclient-basic-linux.x64-12.1.0.2.0.zip /opt/oracle
ADD instantclient-sdk-linux.x64-12.1.0.2.0.zip /opt/oracle

# Install Oracle Instantclient
RUN  unzip /opt/oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && unzip /opt/oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
    && ln -s /opt/oracle/instantclient_12_1/libclntshcore.so.12.1 /opt/oracle/instantclient_12_1/libclntshcore.so \
    && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
    && rm -rf /opt/oracle/*.zip

#RUN LD_LIBRARY_PATH=/opt/oracle/instantclient_12_1:/usr/local/lib/php/extensions/no-debug-non-zts-20170718/
ENV LD_LIBRARY_PATH  /opt/oracle/instantclient_12_1:${LD_LIBRARY_PATH}

# Install Oracle extensions
RUN echo 'instantclient,/opt/oracle/instantclient_12_1/' | pecl install oci8 \
    && docker-php-ext-enable \
        oci8 \
    && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_12_1,12.1 \
    && docker-php-ext-install \
        pdo_oci

# Timezone
RUN echo "date.timezone = Asia/Ho_Chi_Minh" > /usr/local/etc/php/php.ini
RUN echo "upload_max_filesize = 1024M" >> /usr/local/etc/php/php.ini
RUN echo "post_max_size = 1024M" >> /usr/local/etc/php/php.ini

# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE "9000"
