# docker build --no-cache -t php-devmach .

FROM php:7-fpm-alpine3.7

LABEL maintainer='Aydin "devmach" Ulf <root@devmach.com>'

RUN apk upgrade --update && \
    apk --no-cach add \
        postgresql-dev

RUN docker-php-ext-install bcmath iconv pdo_pgsql pgsql shmop \
    && docker-php-ext-enable bcmath iconv pdo_pgsql pgsql shmop

# source: https://github.com/docker-library/wordpress/blob/591388696848f29bfa627ab38f78678135096f15/php7.2/fpm-alpine/Dockerfile
# install the PHP extensions we need
RUN set -ex; \
	\
	apk add --no-cache --virtual .build-deps \
		libjpeg-turbo-dev \
		libpng-dev \
	; \
	\
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install gd mysqli opcache; \
	\
	runDeps="$( \
		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
			| tr ',' '\n' \
			| sort -u \
			| awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' \
	)"; \
	apk add --virtual .wordpress-phpexts-rundeps $runDeps; \
	apk del .build-deps

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN mkdir -p /usr/share/nginx/html \
    && chown -R www-data:www-data /usr/share/nginx/html