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


 
#RUN composer install 
RUN composer install --no-dev --optimize-autoloader

COPY ./bin/wait-for-it.sh /usr/local/bin/wait-for-it.sh
RUN chmod +x /usr/local/bin/wait-for-it.sh

RUN php bin/console lexik:jwt:generate-keypair \
    --no-interaction \
    --skip-if-exists

# Clear cache
# RUN rm -rf /var/cache/apk/*
# RUN rm -rf /var/www/app/var
# RUN rm -rf /var/www/app/public/bundles/

# RUN mkdir /var/www/api
# RUN mkdir /var/www/api/public
#RUN certbot --nginx -d api.localhost

#   && chmod -R 755 /var/www/api/public
    # Ensure runtime directories exist with correct permissions
# Expose the port on which PHP-FPM runs
EXPOSE 9000

COPY ./bin/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
