# Menggunakan official PHP image
FROM php:8.0-fpm

# Menginstal dependencies
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
    libzip-dev

# Menginstall extensions PHP yang diperlukan
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

# Menginstall Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Menyiapkan direktori kerja
WORKDIR /var/www

# Copy aplikasi Laravel
COPY . .

# Menjalankan Composer untuk menginstall dependencies Laravel
RUN composer install

# Mengcopy konfigurasi PHP
COPY .docker/php/conf.d /usr/local/etc/php/conf.d

# Memberikan hak akses yang diperlukan
RUN chown -R www-data:www-data /var/www
RUN chmod -R 755 /var/www/storage

# Expose port 9000 dan menjalankan PHP-FPM
EXPOSE 9000
CMD ["php-fpm"]
