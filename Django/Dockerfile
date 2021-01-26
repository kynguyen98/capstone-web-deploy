FROM alpine:3.9
MAINTAINER john "john@gmail.com"
ENV REFRESHED_AT 2021-02-01

RUN apk add --no-cache --virtual .build-deps \
    gcc \
    python3-dev \
    musl-dev \
    postgresql-dev \
    py3-virtualenv \
    py3-pip \
    bash 
 #   && apk del --no-cache .build-deps

COPY requirements.txt /requirements.txt

RUN virtualenv /.cp && /.cp/bin/pip install --no-cache-dir -r requirements.txt 

VOLUME /capstone
WORKDIR /capstone

EXPOSE 80 8080 8000 443 5432
