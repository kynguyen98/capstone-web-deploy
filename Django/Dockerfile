FROM alpine:3.7

MAINTAINER john "john@gmail.com"

ENV REFRESHED_AT 2021-02-01
ENV PYTHON_ENV .cp
ADD requirements.txt ./

RUN apk update \
&& apk add --no-cache --virtual .build-deps \
gcc \
musl-dev \
py3-virtualenv \
py3-pip \
python3-dev \
postgresql-dev \
&& virtualenv /$PYTHON_ENV \
&& /.cp/bin/pip install --no-cache-dir -r requirements.txt \
&& apk del .build-deps

VOLUME ["/capstone", "$PYTHON_ENV"]

WORKDIR /capstone

CMD ["tail", "-f", "/dev/null"]

EXPOSE 80 8080 8000 443 5432
