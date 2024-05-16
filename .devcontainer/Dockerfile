FROM ubuntu:22.04

ENV COMPOSER_ALLOW_SUPERUSER=1

# install some package. git, unzip & zip are for composer
RUN apt-get update \
    && apt-get install -qy git gnupg locales nano software-properties-common unzip zip

# install locales FR
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
    && echo "fr_FR.UTF-8 UTF-8" >> /etc/locale.gen \
    && locale-gen
ENV LANG=fr_FR.UTF-8
ENV LC_ALL=fr_FR.UTF-8
ENV TZ=Europe/Paris

# Install apache
RUN apt-get install -y apache2 apache2-utils \
    && a2enmod rewrite \
    && a2enmod headers

# Install php 8.1, Composer and Symfony
RUN add-apt-repository ppa:ondrej/php \
#     && echo 'deb http://packages.dotdeb.org jessie all' | tee -a /etc/apt/sources.list \
#     && echo 'deb-src http://packages.dotdeb.org jessie all' | tee -a /etc/apt/sources.list \
#     && cd /tmp \
#     && curl https://www.dotdeb.org/dotdeb.gpg > dotdeb.gpg \
#     && apt-key add dotdeb.gpg \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y php8.1 php8.1-readline \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        php8.1-cli php8.1-curl php8.1-dev php8.1-gd php8.1-intl \
        php8.1-mbstring php8.1-mysql php8.1-opcache php8.1-xml php8.1-zip \
    && echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony5/bin/symfony /usr/local/bin

# Install OpenJDK 11 JRE
RUN apt-get update \
    && apt-get install -y openjdk-11-jre

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

CMD tail -f /dev/null
