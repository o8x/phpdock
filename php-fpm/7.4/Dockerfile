FROM php:7.4.11-fpm-buster

# 拷贝物资
COPY xdebug-2.9.0.tgz /tmp
COPY conf.d/source.list /etc/apt/sources.list
COPY conf.d/* /usr/local/etc/php/conf.d/

WORKDIR /tmp

# 基础依赖
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libjpeg-dev \
        libxml2-dev \
        libssl-dev \
        libbz2-dev \
        libcurl4-gnutls-dev

# Xdebug
RUN mkdir xdebug \
    && tar -xf xdebug-2.9.0.tgz -C xdebug --strip-components=1 \
    && (cd xdebug && phpize && ./configure && make && make install) \
    && docker-php-ext-enable xdebug

# 其他扩展
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install sockets \
    && docker-php-ext-install curl \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-source delete

# 复制 php.ini
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 配置日志输出
RUN mkdir /var/log/php-fpm/ && \
	touch /var/log/php-fpm/error.log && \
	touch /var/log/php-fpm/access.log && \
    rm -rf /usr/local/etc/php-fpm.d/docker.conf && \
    ln -sf /dev/stdout /var/log/php-fpm/access.log && \
    ln -sf /dev/stderr /var/log/php-fpm/error.log

COPY www.conf /usr/local/etc/php-fpm.d/docker.conf

# 打印 php 版本
RUN echo "\n\033[44;30m$(php --version)\033[0m"
