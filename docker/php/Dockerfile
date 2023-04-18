# 最新バージョン確認
FROM php:8.2.5-fpm-alpine
#FROM php:7.3-fpm-alpine
LABEL maintainer="higuchi"

# tinker(psysh)
ARG PSYSH_DIR=/usr/local/share/psysh
ARG PHP_MANUAL_URL=http://psysh.org/manual/ja/php_manual.sqlite

# timezone
ARG TZ

# composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer

RUN set -eux && \
  apk update && \
  apk add --update --no-cache --virtual=.build-dependencies \
    autoconf \
    gcc \
    g++ \
    make \
    tzdata \
    git \
    linux-headers \
    oniguruma-dev && \
  apk add --update --no-cache \
    icu-dev \
    bash \
    libzip-dev \
    supervisor && \
  #OSの設定
  cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  echo ${TZ} > /etc/timezone && \
  #Laravel関連のインストール
  pecl install xdebug && \
  docker-php-ext-install intl pdo_mysql mbstring zip bcmath && \
  docker-php-ext-enable xdebug && \
  mkdir $PSYSH_DIR && wget $PHP_MANUAL_URL -P $PSYSH_DIR && \
  curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer && \
  composer config -g repos.packagist composer https://packagist.jp

#supervisor用設定ファイル設置
COPY supervisord.conf /etc/supervisord.conf
COPY app.ini /etc/supervisord.d/app.ini

#supervisor起動
CMD supervisord -c /etc/supervisord.conf
