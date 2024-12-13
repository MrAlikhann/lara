FROM php:8.1-fpm

# Устанавливаем системные зависимости
RUN apt-get update && apt-get install -y \
      apt-utils \
      libpq-dev \
      libpng-dev \
      libzip-dev \
      zip unzip \
      git && \
      docker-php-ext-install pdo_mysql && \
      docker-php-ext-install bcmath && \
      docker-php-ext-install gd && \
      docker-php-ext-install zip && \
      apt-get clean && \
      rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Копируем php.ini
COPY ./_docker/app/php.ini /usr/local/etc/php/conf.d/php.ini

# Устанавливаем Composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- \
    --filename=composer \
    --install-dir=/usr/local/bin

# Устанавливаем рабочую директорию
WORKDIR /var/www

# Копируем все файлы проекта в контейнер
COPY . .

# Устанавливаем зависимости с помощью Composer
RUN composer install --no-dev --optimize-autoloader

# Устанавливаем права для папок, требующих записи (например, storage, bootstrap/cache)
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache
RUN chmod -R 775 /var/www/storage /var/www/bootstrap/cache

# Открываем порт для веб-сервера
EXPOSE 9000

# Запускаем PHP-FPM
CMD ["php-fpm"]
