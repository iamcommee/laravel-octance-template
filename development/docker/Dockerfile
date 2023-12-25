FROM openswoole/swoole:22.1.2-php8.1-alpine

ARG XDEBUG_VER=3.1.5
ARG IMAGICK_VER=3.7.0
ARG COMPOSER_VERSION=2.4.3

# |--------------------------------------------------------------------------
# | Composer
# |--------------------------------------------------------------------------
# |
# | Installs Composer to manage PHP dependencies
# |
RUN curl -O https://getcomposer.org/download/${COMPOSER_VERSION}/composer.phar && \
    chmod 755 composer.phar && \
    mv composer.phar /usr/local/bin/composer

# |--------------------------------------------------------------------------
# | Required dependencies for laravel
# |--------------------------------------------------------------------------
# |
# | Installs pkgs
# | https://laravel.com/docs/9.x/deployment#server-requirements
# |
RUN apk add --no-cache \
		libpq \
		imagemagick-libs \
		ffmpeg \
		jpegoptim \
		optipng \
		pngquant \
		gifsicle && \
	apk add --no-cache --virtual .build-deps \
		${PHPIZE_DEPS} \
		postgresql-dev \
		imagemagick-dev \
		libjpeg-turbo-dev \
		libpng-dev \
		freetype-dev && \
	docker-php-ext-configure gd \
		--with-freetype --with-jpeg && \
	docker-php-ext-install \
		bcmath \
        opcache \
		pdo_pgsql \
		exif \
		gd && \
	pecl install xdebug-${XDEBUG_VER} && \
	pecl install imagick-${IMAGICK_VER} && \
	docker-php-ext-enable xdebug imagick && \
	apk del .build-deps

RUN apk add php81-pecl-imagick

RUN docker-php-ext-configure pcntl --enable-pcntl \
  && docker-php-ext-install \
    pcntl

RUN apk add npm

# |--------------------------------------------------------------------------
# | App config
# |--------------------------------------------------------------------------
# |
# |
# |
COPY xdebug.ini /usr/local/etc/php/conf.d
COPY php.ini /usr/local/etc/php/
COPY opcache.ini /usr/local/etc/php/conf.d

WORKDIR /code
