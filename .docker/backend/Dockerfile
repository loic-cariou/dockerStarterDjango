FROM python:3.9.6-buster

RUN pip install uwsgi
RUN pip install pipenv

COPY etc/apt/apt.conf.d/proxy.conf /etc/apt/apt.conf.d/proxy.conf

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libatlas-base-dev\
        gfortran\
        nginx \
        supervisor \
        vim \
        git \
        sudo \
        ssh \
        unzip \
        gettext \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN useradd --no-create-home nginx
RUN rm /etc/nginx/sites-enabled/default
RUN rm -r /root/.cache

COPY etc/nginx/nginx.conf /etc/nginx/nginx.conf
COPY etc/sudoers.d/sudo /etc/sudoers.d/sudo
COPY etc/uwsgi/uwsgi.ini /etc/uwsgi/uwsgi.ini
COPY etc/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

COPY ssh/id_ed25519 /root/.ssh/id_rsa
RUN chmod 400 /root/.ssh/id_rsa

ARG HOST_REPO
RUN ssh-keyscan -t rsa ${HOST_REPO} > /root/.ssh/known_hosts

ARG CLONE_REPO
RUN git clone ${CLONE_REPO} /home/app/application

RUN virtualenv /home/app/application/.venv && VIRTUAL_ENV=/home/app/application/.venv && cd /home/app/application/ && pipenv install --dev

EXPOSE 80

CMD ["/usr/bin/supervisord"]