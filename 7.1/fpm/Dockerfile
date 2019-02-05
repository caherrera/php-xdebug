FROM php:7.1-fpm

ARG BUILD_DATE
ARG VCS_REF

ENV COMPOSER_ALLOW_SUPERUSER 1

LABEL Maintainer="Carlos <caherrera@gmail.com>" \
      Description="Container based on php:7.1-fpm with xdebug enabled." \
      org.label-schema.name="carlositline/php-xdebug:7.1-fpm" \
      org.label-schema.description="php 7.2 container based on php-mssql:7.2-fpm with pdo_mssql xDebug enabled & composer installed." \
      org.label-schema.build-date=$BUILD_DATE \
    #   org.label-schema.vcs-url="https://github.com/caherrera/php-7.2-mssql-xdebug-alpine.git" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.schema-version="1.0.0"

RUN set -ex \
    && cd /tmp \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && rm -rf /tmp/* 

ENV ACCEPT_EULA "Y" 

RUN apt-get update
RUN apt-get install -y gnupg2 apt-transport-https unzip
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN set -ex \
&& apt-get install -y msodbcsql17 mssql-tools unixodbc unixodbc-dev locales locales-all libfreetype6-dev libjpeg62-turbo-dev libpng-dev libxml2-dev libldap2-dev \
&& echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile \
&& echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc  
RUN pecl install sqlsrv && pecl install pdo_sqlsrv 
RUN pecl install xdebug
RUN pecl install redis
RUN docker-php-ext-enable redis pdo_sqlsrv sqlsrv;

RUN curl https://phar.phpunit.de/phpunit-7.phar --output /usr/local/bin/phpunit && chmod +x /usr/local/bin/phpunit


RUN docker-php-ext-install -j$(nproc) iconv
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install ldap
RUN docker-php-ext-install zip
RUN docker-php-ext-install soap
RUN docker-php-ext-install sockets
# RUN docker-php-ext-install bz2
#RUN docker-php-ext-install spl


RUN apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

ENV LC_ALL ${LC_ALL:-"es_CL.UTF-8"}
ENV LANG ${LANG:-"es_CL.UTF-8"}
ENV LANGUAGE ${LANGUAGE:-"es_CL.UTF-8"}

# USER www-data