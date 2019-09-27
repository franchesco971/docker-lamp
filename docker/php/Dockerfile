FROM php:7.2-fpm-stretch
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libxml2-dev

RUN curl -sS https://getcomposer.org/installer | php && \
        mv composer.phar /usr/local/bin/composer && \
        chmod +x /usr/local/bin/composer

RUN docker-php-ext-install -j$(nproc) iconv 
RUN docker-php-ext-install mysqli pdo pdo_mysql xml
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd 
RUN pecl install redis-4.0.1 \
    && pecl install xdebug-2.6.0 \
    && docker-php-ext-enable redis xdebug

RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
	} > /usr/local/etc/php/conf.d/opcache-recommended.ini

RUN { \
                echo '[xdebug]'; \
                echo 'xdebug.default_enable = 0'; \
        } >> /usr/local/etc/php/conf.d/xdebug.ini


RUN { \
        echo 'error_reporting  =  E_ALL'; \
        echo 'log_errors = On'; \
        echo 'display_errors = On'; \
        echo 'include_path = ".:/usr/share/php:/usr/local/apache2/htdocs/ws_engine"'; \
    } > /usr/local/etc/php/php.ini

CMD ["php-fpm"]
EXPOSE 9000
