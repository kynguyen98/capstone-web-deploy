FROM httpd:2.4

ENV APACHE_SOCKET=/var/run/apache2 \
PYTHON_ENV=.env \
PYTHON_PATH=capstone
RUN apt update \
&& apt install -y \
libapache2-mod-wsgi-py3 \
&& mkdir -p $APACHE_SOCKET

VOLUME ["$PYTHON_ENV", "$PYTHON_PATH"]

EXPOSE 80 8080 8000 443
