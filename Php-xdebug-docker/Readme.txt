**********PHP+XDEBUG CON DOCKER*************

********************************************
En este modelo de docker configuraremos el 
dockerfile, docker-compose.yml, y el xdebug.ini
mas el archivo lunch.json y describiremos las 
configuraciones comentadas en cada archivo:

****************Dockerfile******************
 # Install and enable Xdebug
RUN pecl install xdebug-3.1.6 && \
    docker-php-ext-enable xdebug

# Copy Xdebug configuration
COPY xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# Copy your code to the Apache web root
ADD tu_codigo /var/www/html

# Set ownership of the code directory to the Apache user
# RUN chown -R www-data:www-data /var/www/html

**************docker-compose.yml***********
version: '3.1'
services:
  webserver:
    build:
    #con estas dos lineas inferiores mandamos contruir 
    #la imagen necesaria de php 
      context: .
      dockerfile: Dockerfile 
    ports:
      - '8081:80'
    volumes:
    #es necesario exponer como volumenes las dos carpetas 
    #para poder copiarlas desde el docker file 
      - './tu_codigo:/var/www/html:rw'
      - './xdebug.ini:/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini'
    extra_hosts:
      - host.docker.internal:host-gateway


***************xdebug.ini*****************
zend_extension=xdebug

[xdebug]
xdebug.mode=develop,debug
xdebug.client_host=host.docker.internal
xdebug.start_with_request=yes


*************launch.json******************
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                   "/var/www/html/": "${workspaceFolder}/tu_codigo"
            }
      },
    ]
}