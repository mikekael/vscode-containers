# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.224.2/containers/php/.devcontainer/base.Dockerfile

# [Choice] PHP version (use -bullseye variants on local arm64/Apple Silicon): 8, 8.1, 8.0, 7, 7.4, 7.3, 8-bullseye, 8.1-bullseye, 8.0-bullseye, 7-bullseye, 7.4-bullseye, 7.3-bullseye, 8-buster, 8.1-buster, 8.0-buster, 7-buster, 7.4-buster
ARG VARIANT="8.1-apache-bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/php:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    libpq-dev \
    libicu-dev \
    libxslt-dev \
    libpng-dev \
    librabbitmq-dev

# install and configure pdo_pgsql driver
RUN docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql

# install and enable intl
RUN docker-php-ext-install intl \
    && docker-php-ext-enable intl

# install and enable xsl
RUN docker-php-ext-install xsl \
    && docker-php-ext-enable xsl

# install and enable gd
RUN docker-php-ext-install gd \
    && docker-php-ext-enable gd

# install and enable ampq
RUN pecl install amqp \
    && docker-php-ext-enable amqp

# install symfony cli
RUN echo 'deb [trusted=yes] https://repo.symfony.com/apt/ /' | sudo tee /etc/apt/sources.list.d/symfony-cli.list \
    && sudo apt-get update \
    && sudo apt-get install -y symfony-cli

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1