FROM httpd:2.4

ENV APACHE_SOCKET=/var/run/apache2 \
PYTHON_ENV=.env
RUN apt-get update \
&& apt-get install -y \
libapache2-mod-wsgi-py3 \
&& cp /etc/mime.types /usr/local/apache2/conf/mime.types \
&& rm -rf /var/lib/apt/lists/* \
&& mkdir -p $APACHE_SOCKET

VOLUME ["$PYTHON_ENV"]

EXPOSE 80 8080 8000 443
