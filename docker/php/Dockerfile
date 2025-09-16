FROM php:8.3-fpm-alpine

WORKDIR /var/www/api

# Install system dependencies
RUN apk add --no-cache \
    bash \
    git \
    curl \
    zip \
    unzip \
    libpq-dev \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    freetype-dev \
    libxml2-dev \
    oniguruma-dev \
    libxslt-dev \
    icu-dev \
    libzip-dev \
    zlib-dev \
    g++ \
    make \
    linux-headers \
    postgresql-client

RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) \
        bcmath \
        exif \
        gd \
        intl \
        mysqli \
        opcache \
        pdo \
        pdo_mysql \
        pdo_pgsql \
        soap \
        xsl \
        zip \
        sockets \
        pcntl

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

COPY . .
RUN mkdir -p /var/www/api/var/cache/prod /var/www/api/var/log \
    && chown -R www-data:www-data /var/www/api/var/cache/prod
#RUN chown -R www-data:www-data /var/www/api && chmod -R 755 /var/www/api
#RUN mkdir -p /var/www/api/var/cache /var/www/api/var/log \
 #   && chown -R www-data:www-data /var/www/api/var \
  #  && chmod -R 775 /var/www/api/var \
   # && chown -R www-data:www-data /var/www/api/var/cache \
    #&& chmod -R 775 /var/www/api/var/cache 


 
RUN composer install --no-dev --optimize-autoloader
RUN composer dump-env prod
RUN php bin/console lexik:jwt:generate-keypair \
    --no-interaction \
    --skip-if-exists
RUN php bin/console cache:clear --env=prod
RUN php bin/console cache:warmup --env=prod
# Expose the port on which PHP-FPM runs
EXPOSE 9000

RUN echo "Starting PHP-FPM..."
CMD ["php-fpm"]
