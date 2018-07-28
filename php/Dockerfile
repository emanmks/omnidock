FROM library/ubuntu:18.04

ARG PHP_VERSION=7.2
ENV PHP_VERSIOn ${PHP_VERSION}

LABEL maintainer="Solaiman Mansyur <solaiman.mansyur@gmail.com>"

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

# Start as root
USER root


###########################################################################
# Core System Installation
##########################
RUN apt update && apt install -y software-properties-common && \
    add-apt-repository -y ppa:ondrej/php && apt update && \
    apt-get install -y --allow-downgrades --allow-remove-essential --allow-change-held-packages \
    php${PHP_VERSION}-cli \
    php${PHP_VERSION}-common \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-intl \
    php${PHP_VERSION}-json \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-mysql \
    php${PHP_VERSION}-pgsql \
    php${PHP_VERSION}-sqlite \
    php${PHP_VERSION}-sqlite3 \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-memcached \
    php${PHP_VERSION}-gd \
    php${PHP_VERSION}-dev \
    pkg-config \
    libcurl4-openssl-dev \
    libedit-dev \
    libssl-dev \
    libxml2-dev \
    xz-utils \
    libxtst6 \
    libxrender1 \
    curl \
    supervisor \
    && apt-get clean

###########################################################################
# More PHP Extensions
#####################

#RUN pecl install mongodb && \
#    echo "extension=mongodb.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`


###########################################################################
# Creating a non-root user:
###########################

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

# always run apt update when start and after add new source list, then clean up at end.
RUN groupadd -g ${PGID} omnidock && \
    useradd -u ${PUID} -g omnidock -m omnidock && \
    usermod -p "*" omnidock

###########################################################################
# Composer Installation
#######################

RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer


###########################################################################
# Final Touch
#############

COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

WORKDIR /var/www

EXPOSE 8000

CMD ["supervisord", "-n"]
