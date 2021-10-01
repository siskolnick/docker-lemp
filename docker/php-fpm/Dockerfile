FROM microsoft/mssql-tools as mssql
FROM php:7.4-fpm-alpine3.14

COPY wait-for-it.sh /usr/bin/wait-for-it

RUN chmod +x /usr/bin/wait-for-it

#COPY --from=mssql /opt/microsoft/ /opt/microsoft/
#COPY --from=mssql /opt/mssql-tools/ /opt/mssql-tools/
#COPY --from=mssql /usr/lib/libmsodbcsql-13.so /usr/lib/libmsodbcsql-13.so

RUN set -xe \
    && apk add --no-cache --virtual .persistent-deps \
        freetds \
        unixodbc \
        libstdc++ \
    && apk add --no-cache --virtual .build-deps \
        $PHPIZE_DEPS \
        unixodbc-dev \
        freetds-dev \
    && docker-php-source extract \
    && docker-php-ext-install pdo_dblib \
    && pecl install \
        sqlsrv \
        pdo_sqlsrv \
    && docker-php-ext-enable --ini-name 30-sqlsrv.ini sqlsrv \
    && docker-php-ext-enable --ini-name 35-pdo_sqlsrv.ini pdo_sqlsrv \
    && docker-php-source delete \
    && apk del .build-deps 
    
RUN apk update && apk upgrade\
    && apk add --no-cache gmp-dev curl libbz2 zip libzip-dev php7-zip php7-bz2 php7-pdo php7-pgsql php7-bcmath php7-zmq php7-curl bash php7-pear php7-imagick openssh libmemcached-dev\
    libtool \
    postgresql-dev \
    libpng-dev \
    imagemagick-c++ \
    imagemagick-dev \
    libmcrypt-dev \
    libxml2-dev \
    yaml-dev \
    bzip2 \
    aspell-dev \
    autoconf \
    build-base \
    linux-headers \
    libaio-dev \
    zlib-dev \
    git \
    subversion \
    freetype-dev \
    libjpeg-turbo-dev \
    libmcrypt-dev \
    bzip2-dev \
    libstdc++ \
    libxslt-dev \
    openldap-dev \
    hiredis-dev \
    make \
    unzip \
    ffmpeg \
    wget \
    icu-dev \
    && echo 'alias sf="php bin/console"' >> ~/.bashrc
RUN apk add --update --no-cache --virtual .build-dependencies $PHPIZE_DEPS \
        && pecl install apcu \
        && docker-php-ext-enable apcu \
        && pecl clear-cache \
        && apk del .build-dependencies

#RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install soap gmp ldap mysqli pdo_mysql gd bcmath bz2 pdo pdo_pgsql simplexml opcache sockets pcntl xsl pspell xml
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip
#RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
#RUN docker-php-ext-configure gd --with-png=/usr/include/ --with-jpeg=/usr/include/ --with-freetype=/usr/include/
RUN docker-php-ext-configure gd --with-jpeg --with-freetype
RUN \
    docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
    && docker-php-ext-configure mysqli --with-mysqli=mysqlnd \
    && docker-php-ext-install pdo_mysql
COPY --from=composer /usr/bin/composer /usr/bin/composer

WORKDIR /var/www

#CMD composer install ; wait-for-it database:3306 -- bin/console doctrine:migrations:migrate ;  php-fpm 

EXPOSE 9000

