FROM python:2

MAINTAINER WireCloud Team <wirecloud@conwet.com>

# Install wirecloud & dependencies
RUN pip install "wirecloud<0.10.0"

RUN apt-get -y update
RUN apt-get install -y apache2 libapache2-mod-wsgi

COPY ["apache-config.conf", "/etc/apache2/sites-enabled/000-default.conf"]

RUN adduser --system --group --shell /bin/bash wirecloud

WORKDIR /opt

RUN wirecloud-admin startproject wirecloud_instance --quick-start
RUN chown -R wirecloud:wirecloud wirecloud_instance

WORKDIR /opt/wirecloud_instance
RUN sed -i "s/SECRET_KEY = '[^']\+'/SECRET_KEY = 'TOCHANGE_SECRET_KEY'/g" wirecloud_instance/settings.py

# The volume must be created after running the wirecloud-admin command
VOLUME /opt/wirecloud_instance

WORKDIR /opt/wirecloud_instance

EXPOSE 80

COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
