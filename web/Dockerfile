FROM php:7.4-apache

RUN apt-get update && apt-get install -y locales apt-transport-https \
        unixodbc libgss3 odbcinst devscripts debhelper dh-exec dh-autoreconf libreadline-dev libltdl-dev \
        tdsodbc unixodbc-dev wget unzip libbz2-dev \
        libfreetype6-dev libmcrypt-dev libjpeg-dev libpng-dev \
        apache2 apache2-utils libssl-dev && \
    rm -rf /var/lib/apt/lists/*

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    ACCEPT_EULA=Y apt-get install -y --no-install-recommends msodbcsql17 mssql-tools && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr \
    && docker-php-ext-install -j$(nproc) pdo_odbc \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable gd pdo_odbc

COPY QuickDbTest.php /var/www/html/

WORKDIR /var/www/html

CMD ["apache2-foreground"]
