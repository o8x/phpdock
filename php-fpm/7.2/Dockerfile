FROM php:7.2-fpm-alpine

COPY ./swoole_loader72.so /opt/swoole_loader72.so
WORKDIR /tmp

# 复制 php.ini
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 拷贝配置文件
COPY conf.d/* /usr/local/etc/php/conf.d/
