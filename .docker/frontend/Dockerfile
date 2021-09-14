FROM debian:10.9-slim

COPY etc/apt/apt.conf.d/proxy.conf /etc/apt/apt.conf.d/proxy.conf

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        gnupg \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*


ARG NODEJS_VERSION=14.x
RUN echo "deb http://deb.nodesource.com/node_${NODEJS_VERSION} buster main" > /etc/apt/sources.list.d/nodejs.list \
    && apt-key adv --fetch-keys http://deb.nodesource.com/gpgkey/nodesource.gpg.key


RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        nginx \
        runit \
        git \
        make \
        nodejs \
        sudo \
        ssh \
        vim \
        unzip \
        supervisor \
    && apt-get clean \
    && npm install -g yarn \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*


RUN useradd --no-create-home nginx
RUN rm /etc/nginx/sites-enabled/default

COPY etc/nginx/nginx.conf /etc/nginx/nginx.conf
COPY etc/sudoers.d/sudo /etc/sudoers.d/sudo
COPY etc/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

COPY ssh/id_ed25519 /root/.ssh/id_rsa
RUN chmod 400 /root/.ssh/id_rsa

ARG HOST_REPO
RUN ssh-keyscan -t rsa ${HOST_REPO} > /root/.ssh/known_hosts

ARG CLONE_REPO
RUN git clone ${CLONE_REPO} /home/app/application

RUN cd /home/app/application/ && yarn install && yarn build

EXPOSE 80

CMD ["/usr/bin/supervisord"]