FROM phpdockerio/php74-fpm:latest


ENV REFRESHED_AT 2020-03-15 10:10:10
ENV TZ "Asia/Shanghai"
ENV INSTALL_LIB_DEP="wget zip unzip"
RUN apt-get update
RUN apt-get install -y $INSTALL_LIB_DEP
RUN apt-get install -y -q  git rake ruby-ronn zlib1g-dev libcurl4-openssl-dev && apt-get clean
RUN apt-get install -y cron vim procps
RUN apt install -y libxml2-dev

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive

# Install selected extensions and other stuff
RUN apt-get update \
    && apt-get -y --no-install-recommends install  php-memcached php7.4-mysql php7.4-pgsql php-redis php7.4-sqlite3 php-xdebug php7.4-bcmath php7.4-bz2 php7.4-dba php7.4-enchant php7.4-gd php-gearman php7.4-gmp php-igbinary php-imagick php7.4-imap php7.4-interbase php7.4-intl php7.4-ldap php-mongodb php-msgpack php7.4-odbc php7.4-phpdbg php7.4-pspell php-raphf php7.4-snmp php7.4-soap php-ssh2 php7.4-sybase php-tideways php7.4-tidy php7.4-xmlrpc php7.4-xsl php-yaml php-zmq \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install git
RUN apt-get update \
    && apt-get -y install git \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install composer
RUN apt-get update \
    && apt-get -y install composer \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install vim
RUN apt-get update \
    && apt-get -y install vim \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Install phpize
RUN apt-get update \
    && apt-get -y install php7.0-dev \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN set -ex \
        && cd /opt \
        && wget https://github.com/phpredis/phpredis/archive/3.0.0.zip -O redis-3.0.0.zip \
        && unzip redis-3.0.0.zip \
        && cd phpredis-3.0.0 \
        && phpize \
        && ./configure \
        && make && make install \
        && echo "extension=redis.so" | tee /etc/php/7.4/fpm/conf.d//redis.ini

RUN set -ex \
        && cd /opt \
        && wget https://github.com/laruence/yaf/archive/yaf-3.0.8.zip -O yaf-3.0.8.zip \
        && unzip yaf-3.0.8.zip \
        && cd yaf-yaf-3.0.8 \
        && phpize \
        && ./configure \
        && make && make install \
        && sh -c 'echo "extension=yaf.so";' | tee /etc/php/7.4/fpm/conf.d//yaf.ini  \
        && sh -c 'echo "[yaf]"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.environ = test"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.library = ../library"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.cache_config = 0"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.name_suffix = 1"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.use_namespace = 0"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.forward_limit = 5"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini \
        && sh -c 'echo "yaf.use_spl_autoload = 0"' | tee -a /etc/php/7.4/fpm/conf.d//yaf.ini

RUN apt-get install libssl-dev -y \
        &&  rm -rf /tmp/pear

#RUN apt-get update -y && \
#    apt-get install -y libmcrypt-dev && \
#    pecl channel-update pecl.php.net && \
#    pecl install mcrypt-1.0.1 && \
#    docker-php-ext-enable mcrypt

RUN set -ex \
    && apt-get update \
    && apt-get upgrade \
    && apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install opcache \
    && docker-php-ext-install dom \
    && docker-php-ext-install simplexml \
    && docker-php-ext-install xml \
    && docker-php-ext-install xmlwriter \
    && docker-php-ext-install xmlrpc

WORKDIR "/application"
RUN chmod 777 /application
