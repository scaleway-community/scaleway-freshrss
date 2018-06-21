## -*- docker-image-name: "scaleway/freshrss:latest" -*-
ARG SCW_ARCH

FROM scaleway/ubuntu:${SCW_ARCH}-xenial

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)

# Prepare rootfs for image-builder
RUN /usr/local/sbin/builder-enter

# Install and configure some apps
RUN apt-get update \
    && apt-get install -y \
       apache2 \
       php \
       php-curl \
       php-gmp \
       php-intl \
       php-mbstring \
       php-sqlite3 \
       php-xml \
       php-zip \
       libapache2-mod-php \
       unzip \
    && a2enmod headers expires rewrite ssl

# Download FreshRSS and set it up
RUN cd /usr/share/ \
    && wget https://github.com/FreshRSS/FreshRSS/archive/master.zip \
         -O freshrss.zip \
    && unzip freshrss.zip \
    && mv FreshRSS-master FreshRSS \
    && chown -R :www-data ./FreshRSS \
    && chmod -R g+r ./FreshRSS \
    && chmod -R g+w ./FreshRSS \
    && chmod -R g+w ./FreshRSS/data/ \
    && ln -sr /usr/share/FreshRSS/p /var/www/html/FreshRSS

# Copy files
COPY overlay ./

# Create server specific doc in /root/ for first login
RUN systemctl enable create-doc

# Symlink doc readme to /root/
RUN ln -s /usr/share/doc/scaleway/freshrss/README /root/README

# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
