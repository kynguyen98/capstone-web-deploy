FROM alpine:3.7
MAINTAINER john "john@123.com"

ENV REFRESHED_AT 2021-02-01
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ADD requirements.txt ./
USER root
RUN apk update && \
apk add --no-cache --virtual .build-deps \
gcc \
musl-dev \
py3-virtualenv \
py3-pip \
&& apk add --no-cache --virtual \
python3-dev \
apache2 \
apache2-mod-wsgi \
&& apk add --no-cache postgresql-dev \
bash \
&& virtualenv /.cp \
&& /.cp/bin/pip install --no-cache-dir -r requirements.txt \
&& apk del --no-cache .build-deps \
&& mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR \
&& echo "Include /etc/apache2/conf/capstone.conf" \
&& echo "LoadModule wsgi_module modules/mod_wsgi-py27.so"

VOLUME /capstone
WORKDIR /capstone
ADD capstone.conf /etc/apache2/conf/
EXPOSE 80 443
#ENTRYPOINT ["/usr/sbin/httpd"]
#CMD ["-D", "FOREGROUND"]
CMD ["./run.sh"]
