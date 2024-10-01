FROM php:8.3-apache

# 1. Install development packages and clean up apt cache.
RUN apt-get update && apt-get install -y \
    curl \
    g++ \
    git \
    libbz2-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libpng-dev \
    libreadline-dev \
    libzip-dev \
    sudo \
    unzip \
    zip \
# Install wkhtmltopdf dependencies
    libfontconfig1 \
    libxrender1 \
    xfonts-75dpi \
    xfonts-base \
    && rm -rf /var/lib/apt/lists/*


# 2. Apache configs + document root.
RUN echo "ServerName projet-app.local" >> /etc/apache2/apache2.conf

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# 3. mod_rewrite for URL rewrite and mod_headers for .htaccess extra headers like Access-Control-Allow-Origin-
RUN a2enmod rewrite headers

# 4. Start with base PHP config, then add extensions.
COPY php.ini "$PHP_INI_DIR/php.ini" 

RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    intl \
    opcache \
    pdo_mysql \
    zip

RUN apt-get update && \
    apt-get install -y zlib1g-dev libpng-dev libjpeg-dev

RUN docker-php-ext-configure gd --with-jpeg && \
    docker-php-ext-install gd

RUN apt-get update; \
    apt-get install -y libmagickwand-dev; \
    pecl install imagick; \
    docker-php-ext-enable imagick;

RUN apt-get install -y ghostscript

RUN apt-get update && apt-get install -y imagemagick

# 5. Composer.
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 5.5 Download and install wkhtmltopdf
# RUN curl -LO "http://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.1_1.1.1n-0+deb11u5_amd64.deb" \
#     && dpkg -i libssl1.1_1.1.1n-0+deb11u5_amd64.deb \
#     && rm libssl1.1_1.1.1n-0+deb11u5_amd64.deb

# RUN curl -LO "https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.bullseye_amd64.deb" \
#     && dpkg -i wkhtmltox_0.12.6.1-2.bullseye_amd64.deb \
#     && rm wkhtmltox_0.12.6.1-2.bullseye_amd64.deb


# 6. Create system user to run Composer and Artisan Commands
ARG uid
RUN useradd -G www-data,root -u $uid -d /home/devuser devuser
RUN mkdir -p /home/devuser/.composer && \
    chown -R devuser:devuser /home/devuser

# 7. Install Node.js and npm (latest LTS version)
RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
RUN apt-get install -y nodejs
