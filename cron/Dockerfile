# syntax=docker/dockerfile:1

FROM php:8.0-fpm

RUN apt-get update \
    && apt-get install -y cron libzip-dev \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install zip \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php  --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

COPY ./cron/root /var/spool/cron/crontabs/

RUN crontab /var/spool/cron/crontabs/root

CMD ["cron", "-f"]