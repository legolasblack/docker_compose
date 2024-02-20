FROM php:8.0-apache

LABEL authors="Legolas Lothbrok"

# Install and enable Xdebug
RUN pecl install xdebug-3.1.6 && \
    docker-php-ext-enable xdebug

# Copy Xdebug configuration
COPY xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Copy your code to the Apache web root
ADD tu_codigo /var/www/html

# Set ownership of the code directory to the Apache user
# RUN chown -R www-data:www-data /var/www/html
