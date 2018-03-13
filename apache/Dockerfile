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
    wget

# add repo with php7.2
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list

# install php7.2
RUN apt-get update \
    && apt-get install -y \
    php7.2 \
    php7.2-fpm \
    php7.2-common \
    php7.2-curl \
    php7.2-gd \
    php7.2-json \
    php7.2-mbstring \
    php7.2-mysql \
    php7.2-opcache


# Script to pass MySQL
RUN mkdir -p /var/www/html
COPY index.php /var/www/html

# Copy Supervisor file in to conatianer
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# link apache php-fpm config
RUN mkdir /run/php
RUN ln -s /etc/apache2/conf-available/php7.2-fpm.conf /etc/apache2/conf-enabled

# +++ COPY mitbringen config to sites-enabled

# +++ clone mitbringen repo



# expose MySQL port to a network
EXPOSE 80

# open to mount
VOLUME ["/var/www"]

# start supervisor which will start apache and php-fpm
CMD ["/usr/bin/supervisord"]