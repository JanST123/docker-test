#mitbringen/mysql
FROM debian:jessie

LABEL vendor="mitbringen.net"
LABEL net.mitbringen.version="0.0.1"
LABEL net.mitbringen.component="Apache with PHP7.2-FPM"


# install apache and some dependencies
RUN apt-get update \
    && apt-get install -y \
    apache2 \
    apache2-mpm-prefork \
    apt-transport-https \
    ca-certificates \
    libapache2-mod-fcgid \
    lsb-release \
    supervisor \
    wget \
    apt-transport-https

# add repo with php7.2
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list

# install php7.2
RUN apt-get update \
    && apt-get install -y \
    vim \
    php7.2 \
    php7.2-fpm \
    php7.2-common \
    php7.2-curl \
    php7.2-gd \
    php7.2-json \
    php7.2-mbstring \
    php7.2-mysql \
    php7.2-opcache

# for dev-machine: set www-data UID and GID to 1000 to match system user on shared volumes
RUN usermod -u 1000 www-data
RUN groupmod -g 1000 www-data


# Script to pass www-data
RUN mkdir -p /var/www/html
COPY index.php /var/www/html

# Copy Supervisor file in to conatianer
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# link apache php-fpm config
RUN mkdir /run/php
RUN ln -s /etc/apache2/conf-available/php7.2-fpm.conf /etc/apache2/conf-enabled
RUN ln -s /etc/apache2/mods-available/proxy.conf /etc/apache2/mods-enabled
RUN ln -s /etc/apache2/mods-available/proxy.load /etc/apache2/mods-enabled
RUN ln -s /etc/apache2/mods-available/proxy_fcgi.load /etc/apache2/mods-enabled

# +++ import mysql dump

# +++ COPY mitbringen config to sites-enabled

# +++ clone mitbringen repo



# expose apache port to a network
EXPOSE 80

# open to mount
VOLUME ["/var/www"]

# start supervisor which will start apache and php-fpm
CMD ["/usr/bin/supervisord"]
