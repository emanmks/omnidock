ARG NODE_VERSION=8
FROM library/node:${NODE_VERSION}

LABEL maintainer="Solaiman Mansyur <solaiman.mansyur@gmail.com>"

RUN apt-get update && apt-get -y install supervisor

WORKDIR /var/www

COPY supervisor.conf /etc/supervisor/conf.d/supervisor.conf

ARG NODE_PORT=8800
ENV NODE_PORT ${NODE_PORT}
EXPOSE ${NODE_PORT}

CMD ["supervisord", "-n"]
