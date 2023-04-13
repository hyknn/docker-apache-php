FROM php:7.4-apache

# Copy composer.lock and composer.json
COPY src/composer.lock src/composer.json /var/www/html/

# Set working directory
WORKDIR /var/www/html

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl \
    libzip-dev \
    libonig-dev 

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql \
    mbstring \
    tokenizer \
    zip \
    exif \
    pcntl \
    gd \
    mysqli && docker-php-ext-enable mysqli

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY src/ /var/www/html

# Copy existing application directory permissions
COPY --chown=www:www . /var/www/html

# Change current user to root
USER root

# Enable the mod_rewrite module
RUN a2enmod rewrite

# Change current user to www
USER www

