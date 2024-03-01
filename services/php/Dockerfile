# 使用官方PHP镜像，带有预安装的PHP-FPM
ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm

# 安装依赖和PHP扩展
RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libzip-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip \
    # 安装Redis扩展
    && pecl install redis \
    && docker-php-ext-enable redis

###########################################################################
# Opcache:
###########################################################################

ARG INSTALL_OPCACHE=false

RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# 将你的www目录（包含PHP代码）复制到容器内的工作目录
COPY ./www /var/www/html

# 设置工作目录
WORKDIR /var/www/html

# 暴露9000端口供Nginx使用
EXPOSE 9000

# 启动PHP-FPM服务
CMD ["php-fpm"]
