# Base image
FROM ubuntu:20.04

# Set noninteractive to prevent any prompts during the build
ARG DEBIAN_FRONTEND=noninteractive

# Update packages and install necessary tools
RUN apt-get update && apt-get install -y \
    software-properties-common \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Add Ondřej Surý repository for PHP
RUN add-apt-repository ppa:ondrej/php -y

# Update packages again after adding repository
RUN apt-get update

# Install necessary packages
RUN apt-get install -y nginx curl git git-core unzip supervisor
RUN apt update && apt-get install -y \
                    php8.1-fpm \
                    php8.1-bcmath \
                    php8.1-bz2 \
                    php8.1-cli \
                    php8.1-curl \
                    php8.1-dev \
                    php8.1-gd \
                    php8.1-igbinary \
                    php8.1-imagick \
                    php8.1-imap \
                    php8.1-intl \
                    php8.1-mbstring \
                    php8.1-memcached \
                    php8.1-msgpack \
                    php8.1-mysql \
                    php8.1-pgsql \
                    php8.1-readline \
                    php8.1-redis \
                    php8.1-ssh2 \
                    php8.1-tidy \
                    php8.1-xml \
                    php8.1-xmlrpc \
                    php8.1-zip

RUN echo "<?php phpinfo();?>" >> /var/www/html/info.php
# Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"
# Configure Nginx

RUN rm /etc/nginx/sites-enabled/default
COPY config/nginx.conf /etc/nginx/sites-enabled/default

# Supervisor configuration
COPY config/supervisord.conf /etc/supervisor/conf.d/laravel.conf

# Expose ports
EXPOSE 80

# Start services
CMD ["/bin/sh", "-c", "service nginx start && service php8.1-fpm start && service supervisor start && /etc/init.d/supervisor start"]