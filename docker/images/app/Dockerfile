FROM php:7.3-fpm-alpine

ARG APP_CONTEXT_PATH
ARG DOCKER_IMAGE_CONTEXT_PATH
ARG GROUP_ID=1337
ARG USER_ID=1337

RUN addgroup -g $GROUP_ID -S dockeruser

RUN adduser -S -G dockeruser -u $USER_ID -s /bin/ash dockeruser

COPY --chown=$USER_ID:$GROUP_ID $APP_CONTEXT_PATH /home/dockeruser/app

# PHP gd dependencies
RUN apk add zlib-dev
RUN apk add jpeg-dev
RUN apk add libpng-dev
RUN apk add freetype-dev

# PHP intl dependencies
RUN apk add icu-dev

# PHP soap dependencies
RUN apk add libxml2-dev

# PHP xsl dependencies
RUN apk add libxslt-dev

# PHP zip dependencies
RUN apk add libzip-dev

# PHP curl
RUN apk add curl-dev

# PHP openssl
RUN apk add openssl-dev

RUN docker-php-ext-install bcmath
# RUN docker-php-ext-configure gd --with-freetype --with-jpeg
# RUN docker-php-ext-install gd
RUN docker-php-ext-install intl
RUN docker-php-ext-install soap
RUN docker-php-ext-install xsl
RUN docker-php-ext-install zip
RUN docker-php-ext-install sockets

# Xdebug
RUN apk add --no-cache $PHPIZE_DEPS \
  && pecl install xdebug-3.1.2 \
  && docker-php-ext-enable xdebug

# PHP Redis
RUN apk add --no-cache pcre-dev $PHPIZE_DEPS \
  && pecl install redis \
  && docker-php-ext-enable redis.so

COPY --chown=$USER_ID:$GROUP_ID $DOCKER_IMAGE_CONTEXT_PATH/app/php.ini /usr/local/etc/php/php.ini
#COPY --chown=$USER_ID:$GROUP_ID $DOCKER_IMAGE_CONTEXT_PATH/app/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN touch /var/log/app.log
RUN touch /var/log/fpm-php.www.log

RUN chown -R dockeruser /var && chmod -R 755 /var

# Composer - apk add composer installs incorrect version of php
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
     php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
     php composer-setup.php && \
     php -r "unlink('composer-setup.php');" && \
     mv composer.phar /usr/local/bin/composer;

WORKDIR /home/dockeruser/app

USER dockeruser

EXPOSE 9000

CMD ["sh", "-c", "composer install && php-fpm"]
