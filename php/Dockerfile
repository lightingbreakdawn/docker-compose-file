ARG PHP_VERSION=7.2
FROM php:${PHP_VERSION}-fpm

# replace sources.list
ARG CHANGE_SOURCE=false
RUN if [ ${CHANGE_SOURCE} = true ]; then \
    sed -i 's/deb.debian.org/mirrors.163.com/g' /etc/apt/sources.list \
    && sed -i 's/security.debian.org/mirrors.163.com/g' /etc/apt/sources.list \
;fi

ARG TIMEZONE=Asia/Shanghai
RUN cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
    && echo "${TIMEZONE}" > /etc/timezone

# init
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    wget \
    zip \
    bzip2 \
    git \
    unzip \
    procps \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    libmemcached-dev \
    libmagickwand-dev \
    imagemagick \
    libgmp-dev \
    libzip-dev \
    apt-utils \
    && rm -rf /var/lib/apt/lists/*

# install php extentions
RUN pecl channel-update pecl.php.net \
    && docker-php-ext-configure gd \
    --enable-gd-native-ttf \
    --with-jpeg-dir=/usr/lib \
    --with-freetype-dir=/usr/include/freetype2 \
    && docker-php-ext-install -j$(nproc) zip bz2 bcmath mysqli pdo_mysql gd gmp sockets pcntl sysvmsg sysvsem sysvshm \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && pecl clear-cache
	
# clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

EXPOSE 9000
CMD ["php-fpm"]