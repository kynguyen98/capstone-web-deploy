FROM alpine:3.7
MAINTAINER john "john@123.com"
ENV PYTHON_ENV .cp 
ENV REFRESHED_AT 2021-02-01
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

RUN set -x \
	&& addgroup -g 82 -S www-data \
	&& adduser -u 82 -D -S -G www-data www-data \
&& apk update \
&& apk add --no-cache \
python3-dev \
apache2 \
apache2-mod-wsgi \
postgresql-dev \
&& mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR \
&& mkdir /etc/apache2/conf \
&& echo "Include /etc/apache2/conf/capstone.conf" >> /etc/apache2/httpd.conf \
&& echo "LoadModule wsgi_module modules/mod_wsgi-py27.so" >> /etc/apache2/httpd.conf

VOLUME ["/capstone", "$PYTHON_ENV"]
WORKDIR /capstone
EXPOSE 80 8080 443 5432
#ENTRYPOINT ["/usr/sbin/httpd"]
#CMD ["-D", "FOREGROUND"]
#CMD ["./run.sh"]
