#mitbringen/mysql
FROM debian:jessie

LABEL vendor="mitbringen.net"
LABEL net.mitbringen.version="0.0.1"
LABEL net.mitbringen.component="MySQL"



#define access data for public user
ENV DB_USER mitbringen
ENV DB_PASSWORD 1234
ENV DB_NAME mitbringen
ENV VOLUME_HOME "/var/lib/mysql"

# Get MySQL
# for skipping interactive mysql root password question (we set the root pw empty, but root login is only allowed from localhost)
RUN apt-get update \
    && apt-get install -y apt-utils \
    && { \
        echo debconf debconf/frontend select Noninteractive; \
        echo mysql-server-5.5 mysql-server-5.5/root-pass \
            password ''; \
        echo mysql-server-5.5 mysql-server-5.5/re-root-pass \
            password ''; \
        echo mysql-server-5.5 mysql-community-server/remove-test-db \
            select true; \
    } | debconf-set-selections \
    && apt-get install -y mysql-server-5.5 \
    vim


# Remove pre-installed database
RUN rm -rf /var/lib/mysql/*
# remove stupid config which steals our logs
RUN rm /etc/mysql/conf.d/mysqld_safe_syslog.cnf



# Open MySQL to a world
RUN sed -i -e"s/^bind-address[[:space:]]*=[[:space:]]*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

# Script to pass MySQL
COPY startup.sh /
RUN chmod 755 /*.sh


# expose MySQL port to a network
EXPOSE 3306

# open to mount
VOLUME ["/var/lib/mysql", "/var/log/mysql"]

# start container
CMD ["/bin/bash", "/startup.sh"]
