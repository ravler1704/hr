FROM php:8.2-fpm

RUN apt-get update && \
    apt-get install -y \
        libjpeg62-turbo-dev \
        libfreetype6-dev \
        libpng-dev \
        libjpeg-dev \
        libxml2-dev \
        libcurl4-openssl-dev \
        libldap2-dev \
        libzip-dev \
        wget \
        && \
    apt-get update && \
    apt-get install -y \
        libgd3 \
        libgd-dev \
        libzip-dev \
        zip \
        && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure gd --with-freetype --with-jpeg && \
    docker-php-ext-install \
        pdo_mysql \
        mysqli \
        soap \
        zip \
        gd \
        intl \
        ldap \
        zip \
        opcache \
        && \
    pecl install \
        memcache-8.0 \
        xdebug-3.2.0 \
        && \
    docker-php-ext-enable \
        xdebug \
        memcache \
        && \
    apt-get autoremove && \
    apt-get clean && \
    rm -r /var/lib/apt/lists/*



# Устанавливаем memcached
RUN apt-get update && apt-get install -y \
        libmemcached11 \
        libmemcachedutil2 \
        libmemcached-dev \
        libz-dev \
        git \
    && cd /root \
    && git clone https://github.com/php-memcached-dev/php-memcached \
    && cd php-memcached \
    && phpize \
    && ./configure \
    && make \
    && make install \
    && cd .. \
    && rm -rf  php-memcached \
    && echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini \
    && apt-get remove -y build-essential libmemcached-dev libz-dev \
    && apt-get remove -y libmemcached-dev libz-dev \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Для работы sendmail в паре с MailHog
RUN wget --no-verbose https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 && \
    chmod +x mhsendmail_linux_amd64 && \
    mv mhsendmail_linux_amd64 /usr/local/bin/mhsendmail

# Using the default development configuration
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"
# Overriding default config
COPY php.override.ini $PHP_INI_DIR/conf.d/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


WORKDIR /app
