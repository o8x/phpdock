FROM php:7.3-fpm-alpine

COPY ./xdebug-2.7.2.tgz /tmp
WORKDIR /tmp

# 换源和安装基础依赖
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk add --no-cache autoconf g++ libtool make curl-dev libxml2-dev linux-headers freetype-dev libjpeg-turbo-dev libpng-dev libssh2 libssh2-dev

# 安装 Xdebug
RUN mkdir xdebug \
    && tar -xf xdebug-2.7.2.tgz -C xdebug --strip-components=1 \
    && (cd xdebug && phpize && ./configure && make && make install)

# Swoole
#RUN mkdir swoole \
#    && tar -xf swoole-4.2.1.tgz -C xdebug --strip-components=1 \
#    && (cd swoole && phpize && ./configure && make && make install)

# 启用扩展
# docker-php-ext-enable  用于安装默认扩展，会自动编译和安装
RUN docker-php-ext-enable xdebug

# docker-php-ext-install 用于启用自行编译的扩展，节省了添加so的时间
RUN  docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install exif \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install sockets \
    && docker-php-ext-install curl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd mysqli 

# 复制 php.ini
RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 拷贝配置文件
COPY conf.d/* /usr/local/etc/php/conf.d/
