##
# EchoSwoole PHP Swoole docker image
# @build-example docker build -t esphp/php .
##
FROM php:fpm-alpine

LABEL maintainer="EchoLi <php360@qq.com>"

ENV PHPIZE_DEPS \
        autoconf \
        dpkg-dev dpkg \
        file \
        g++ \
        gcc \
        libc-dev \
        make \
        pkgconf \
        re2c \
        pcre-dev \
        zlib-dev

RUN set -xe \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
    && apk update \
    && apk add --no-cache libstdc++ openssl \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && pecl install swoole \
    && pecl install mongodb \
    && echo "extension=swoole.so" >> /usr/local/etc/php/php.ini \
    && echo "extension=mongodb.so" >> /usr/local/etc/php/php.ini \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man ~/.pearrc \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

VOLUME ["/var/www/html"]

WORKDIR /var/www/html

EXPOSE 9000 9501

CMD ["php-fpm"]