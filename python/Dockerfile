ARG UBUNTU_VERSION=18.04
FROM library/ubuntu:${UBUNTU_VERSION}

LABEL maintainer="Solaiman Mansyur <solaiman.mansyur@gmail.com>"

RUN apt-get update \
  && apt-get install -y python3-pip python3-dev supervisor \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip

###########################################################################
# Creating a non-root user:
###########################

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} omnidock && \
    useradd -u ${PUID} -g omnidock -m omnidock && \
    usermod -p "*" omnidock

###########################################################################
# Final Touch
#############

COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf

# Clean up
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

WORKDIR /var/www/

ARG PYTHON_PORT=5000
ENV PYTHON_PORT ${PYTHON_PORT}
EXPOSE ${PYTHON_PORT}

CMD ["supervisord", "-n"]
