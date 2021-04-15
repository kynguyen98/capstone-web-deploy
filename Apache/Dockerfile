FROM httpd:2.4

ENV APACHE_SOCKET=/var/run/apache2 \
PYTHON_ENV=.env
RUN apt update \
&& apt install -y \
libapache2-mod-wsgi-py3 \
&& mkdir -p $APACHE_SOCKET

VOLUME ["$PYTHON_ENV"]

EXPOSE 80 8080 8000 443
