FROM php:7.1-fpm
RUN docker-php-ext-install pdo_mysql
RUN apt-get update && apt-get install -y \
        libmcrypt-dev \
    && docker-php-ext-install -j$(nproc) mcrypt \
    && docker-php-ext-install -j$(nproc) pdo

RUN apt-get install -y nginx  supervisor && \
    rm -rf /var/lib/apt/lists/*

COPY . /var/www/html
WORKDIR /var/www/html

RUN rm /etc/nginx/sites-enabled/default

COPY /docker/nginx/nginx.conf /etc/nginx/conf.d/default.conf
WORKDIR /var/www/html

RUN php artisan serve --host=0.0.0.0 &

ADD https://getcomposer.org/download/1.6.2/composer.phar /usr/bin/composer
RUN chmod +rx /usr/bin/composer

RUN composer install

RUN cp .env.example .env

