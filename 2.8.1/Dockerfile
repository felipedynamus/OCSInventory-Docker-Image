FROM ubuntu:20.04

LABEL maintainer="contact@ocsinventory-ng.org" \
      version="2.8.1" \
      description="OCS Inventory docker image"

ARG YUM_FLAGS="-y"

ENV APACHE_RUN_USER=apache APACHE_RUN_GROUP=apache \
    APACHE_LOG_DIR=/var/log/httpd APACHE_PID_FILE=/var/run/httpd.pid APACHE_RUN_DIR=/var/run/httpd APACHE_LOCK_DIR=/var/lock/httpd \
    OCS_DB_SERVER=dbsrv OCS_DB_PORT=3306 OCS_DB_USER=ocs OCS_DB_PASS=ocs OCS_DB_NAME=ocsweb \
    OCS_LOG_DIR=/var/log/ocsinventory-server OCS_VARLIB_DIR=/var/lib/ocsinventory-reports/ OCS_WEBCONSOLE_DIR=/usr/share/ocsinventory-reports \
    OCS_PERLEXT_DIR=/etc/ocsinventory-server/perl/ OCS_PLUGINSEXT_DIR=/etc/ocsinventory-server/plugins/ \
    OCS_SSL_ENABLED=0 OCS_SSL_WEB_MODE=DISABLED OCS_SSL_COM_MODE=DISABLED OCS_SSL_KEY=/path/to/key OCS_SSL_CERT=/path/to/cert OCS_SSL_CA=/path/to/ca \
    TZ=Europe/Paris

VOLUME /var/lib/ocsinventory-reports /etc/ocsinventory-server /usr/share/ocsinventory-reports

WORKDIR /tmp

RUN apt-get install wget \
    curl \
    apt-utils \
    tar \
    make \
    apt-get install epel-release ; \
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm ; \
    wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm ; \
    rpm -Uvh remi-release-7.rpm ; \
    apt-config-manager --enable remi-php73 ; \
    apt-get update ; \
    apt-get install perl \
    perl-XML-Simple \
    perl-Compress-Zlib \
    perl-DBI perl-DBD-MySQL \
    perl-Net-IP \
    perl-SOAP-Lite \
    perl-Archive-Zip \
    perl-Mojolicious \
    perl-Plack \
    perl-XML-Entities \
    perl-Switch \
    perl-Apache-DBI \
    Apache2 \
    php74-php \
    php74-php-cli \
    php74-php-cli \
    php74-php-gd \
    php74-php-imap \
    php74-php-pdo \
    php74-php-pear \
    php74-php-mbstring \
    php74-php-intl \
    php74-php-mysqlnd \
    php74-php-xml \
    php74-php-xmlrpc \
    php74-php-pecl-mysql \
    php74-php-pecl-mcrypt \
    php74-php-pecl-apcu \
    php74-php-json \
    php74-php-fpm \
    php74-php-soap \
    php74-php-opcache ;

RUN wget https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/download/2.8.1/OCSNG_UNIX_SERVER_2.8.1.tar.gz && \
    tar xzf OCSNG_UNIX_SERVER_2.8.1.tar.gz ;

WORKDIR /tmp/OCSNG_UNIX_SERVER_2.8.1

RUN cd Apache/ && \
    perl Makefile.PL && \ 
    make && \
    make install ;

WORKDIR /tmp

COPY conf/ /tmp/conf
COPY ./scripts/docker-entrypoint.sh /usr/bin/docker-entrypoint.sh

EXPOSE 80 443

ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
