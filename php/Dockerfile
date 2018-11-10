##
# EchoSwoole PHP Swoole docker image
# @build-example docker build -t esphp/php .
##
FROM php:fpm-alpine

LABEL maintainer="EchoLi <php360@qq.com>"

ENV SWOOLE_VERSION=4.2.6

RUN set -xe \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/' /etc/apk/repositories \
    && apk update \
    && apk add --no-cache pcre-dev zlib-dev libstdc++ openssl openssl-dev libaio-dev nghttp2-dev \
    && apk add --no-cache --virtual .build-deps $PHPIZE_DEPS \
    # Edit php.ini #
    && cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini \
    && sed -i 's/post_max_size = 8M/post_max_size = 100M/g' /usr/local/etc/php/php.ini \
    && sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 100M/g' /usr/local/etc/php/php.ini \
    && sed -i 's/;date.timezone =/date.timezone = PRC/g' /usr/local/etc/php/php.ini \
    && sed -i 's/max_execution_time = 30/max_execution_time = 300/g' /usr/local/etc/php/php.ini \
    && sed -i 's/disable_functions =.*/disable_functions = passthru,exec,system,chroot,chgrp,chown,shell_exec,popen,ini_alter,ini_restore,dl,openlog,syslog,readlink,symlink,popepassthru/g' /usr/local/etc/php/php.ini \
    # install ext
    && docker-php-ext-install mysqli \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install sockets \
    # install swoole
    && cd /tmp \
    && curl -SL "https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz" -o swoole.tar.gz \
    && tar -zxvf swoole.tar.gz \
    && mv swoole-src* swoole \
    && cd swoole \
    && phpize \
    && ./configure --enable-sockets --enable-openssl --enable-http2 --enable-async-redis --enable-mysqlnd \
    && make && make install \
    && echo "extension=swoole.so" >> /usr/local/etc/php/conf.d/swoole.ini \
    # install mongodb
    && pecl install mongodb \
    && echo "extension=mongodb.so" >> /usr/local/etc/php/conf.d/mongodb.ini \
    && apk del .build-deps \
    && rm -rf /var/cache/apk/* /tmp/* /usr/share/man ~/.pearrc \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

VOLUME ["/var/www/html"]
WORKDIR /var/www/html
EXPOSE 9000 9501

CMD ["php-fpm"]