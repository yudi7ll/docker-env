FROM php:7.3-apache

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN install-php-extensions pdo_mysql zip gd

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

# NPM
# RUN apk add composer npm --no-cache
# RUN apt update && apt upgrade -y && apt install -y npm
RUN echo "ServerName localhost:80" >> /etc/apache2/apache2.conf

# enable module to read .htaccess config
RUN a2enmod rewrite

# configure file owner permissions
RUN useradd -g www-data -u 1000 yudi
RUN chown -R yudi:www-data /var/www/html

# clean up
RUN rm /tmp/* -rf

COPY . /var/www/html

CMD ["apache2ctl", "-D", "FOREGROUND"]
