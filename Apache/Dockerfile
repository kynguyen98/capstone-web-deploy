FROM alpine:3.9
MAINTAINER john "john@123.com"
ENV REFRESHED_AT 2021-02-01

USER root
RUN apk update; apk add bash apache2 
VOLUME /var/www/html
WORKDIR /var/www/html

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

ADD httpd.conf /etc/apache2/

RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR 
EXPOSE 80 443
#ENTRYPOINT ["/usr/sbin/httpd"]
#CMD ["-D", "FOREGROUND"]
