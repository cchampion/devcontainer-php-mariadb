FROM php:8.1.13-apache

ENV COMPOSER_ALLOW_SUPERUSER 1

# install some package. git, unzip & zip are for composer
RUN apt update \
    && apt install -qy git gnupg nano software-properties-common unzip zip

# install locales FR
RUN apt install -y locales \
    && rm -rf /var/lib/apt/lists/* \
	&& localedef -i fr_FR -c -f UTF-8 -A /usr/share/locale/locale.alias fr_FR.UTF-8
ENV LANG fr_FR.utf8
ENV LC_ALL fr_FR.utf8
ENV TZ Europe/Paris

# Install apache
#RUN apt update
#RUN apt install -y apache2
#RUN apt install -y apache2-utils
#RUN a2enmod rewrite
#RUN a2enmod headers
#
## Install php 8.1, Composer and Symfony
RUN apt update
RUN add-apt-repository ppa:ondrej/php
##     && echo 'deb http://packages.dotdeb.org jessie all' | tee -a /etc/apt/sources.list \
##     && echo 'deb-src http://packages.dotdeb.org jessie all' | tee -a /etc/apt/sources.list \
##     && cd /tmp \
##     && curl https://www.dotdeb.org/dotdeb.gpg > dotdeb.gpg \
##     && apt-key add dotdeb.gpg \
RUN apt update
#RUN DEBIAN_FRONTEND=noninteractive apt install -y php8.1 php8.1-readline
RUN DEBIAN_FRONTEND=noninteractive apt install -y \
        php8.1-cli php8.1-curl php8.1-dev php8.1-gd php8.1-intl \
        php8.1-mbstring php8.1-mysql php8.1-opcache php8.1-xml php8.1-zip
RUN echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN apt clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin

# Install OpenJDK 11 JRE
RUN apt update \
    && apt install -y openjdk-11-jre

# Install nvm, Node 20.8.1, yarn
RUN mkdir /usr/local/nvm
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 20.8.1
RUN curl https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash \
    && . $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default \
    && npm install -g yarn

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# Copy files in docker container
COPY etc/ /etc/
COPY var/ /var/

#CMD tail -f /dev/null
CMD ["apache2-foreground"]

