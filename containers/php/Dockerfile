FROM php:8.4-fpm

ARG USER='1000'
ARG USER=${USER_ID}
ENV USER ${USER_ID}

ARG GROUP='1000'
ARG GROUP=${GROUP_ID}
ENV GROUP ${GROUP_ID}

ARG PROJECT_PREFIX='develop'
ARG PROJECT_PREFIX=${PROJECT_PREFIX}
ENV PROJECT_PREFIX=${PROJECT_PREFIX}

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

# Установка Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN pecl install xdebug && docker-php-ext-enable xdebug

RUN apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        curl \
        bash \
        git \
        unzip \
    && install-php-extensions memcache \
    && docker-php-ext-enable memcache \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
    && docker-php-ext-install mysqli \
    && install-php-extensions ldap \
    && install-php-extensions zip \
    && install-php-extensions exif \
    && install-php-extensions soap

# Настройка прав для Composer (опционально)
RUN chmod +x /usr/bin/composer

ADD ./conf/bitrix.ini "/usr/local/etc/php/conf.d/bitrix.ini"
ADD ./conf/www.conf "/usr/local/etc/php-fpm.d/www.conf"

WORKDIR /home/bitrix/www

EXPOSE 9000
