FROM ubuntu:16.04

MAINTAINER Lyovkin Anatoliy <anatoliy.lyovkin@lazy-ants.de>

ENV LC_ALL=C.UTF-8

RUN apt-get update && apt-get --no-install-recommends --no-install-suggests install -y \
    software-properties-common \
    python-software-properties \
    && add-apt-repository ppa:ondrej/php && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get remove php7.0 && \
    apt-get --no-install-recommends --no-install-suggests install -y \
    curl \
    nginx \
    php7.1 \
    php7.1-fpm \
    php7.1-cli \
    php7.1-common \
    ca-certificates \
    gettext && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y --no-install-recommends --no-install-suggests \
    php7.1-mongodb \
    php7.1-curl \
    php7.1-intl \
    php7.1-soap \
    php7.1-xml \
    php7.1-mcrypt \
    php7.1-bcmath \
    php7.1-mysql \
    php7.1-amqp \
    php7.1-mbstring \
    php7.1-ldap \
    php7.1-zip \
    php7.1-gd \
    php7.1-xdebug \
    php7.1-imagick && \
    rm -f /etc/php/7.1/cli/conf.d/*xdebug.ini && \
    rm -f /etc/php/7.1/fpm/conf.d/*xdebug.ini && \
    rm -rf /var/lib/apt/lists/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN ln -sf /dev/stderr /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log \
	&& ln -sf /dev/stderr /var/log/php7.1-fpm.log \
	&& ln -sf /dev/stderr /var/log/php-fpm.log

RUN rm -f /etc/nginx/sites-enabled/*

COPY php/php-fpm.conf.dist /etc/php/7.1/fpm/php-fpm.conf
COPY php/defaults.ini /etc/php/7.1/cli/conf.d/defaults.ini
COPY php/defaults.ini /etc/php/7.1/fpm/conf.d/defaults.ini

RUN mkdir -p /run/php && touch /run/php/php7.1-fpm.sock && touch /run/php/php7.1-fpm.pid
RUN usermod -u 1000 www-data

COPY bootstrap.sh /bootstrap.sh
RUN chmod 755 /bootstrap.sh

EXPOSE 80

CMD ["/bootstrap.sh"]
