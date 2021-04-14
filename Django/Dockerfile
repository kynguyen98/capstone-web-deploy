FROM alpine:3.7

ENV PYTHON_ENV=.env

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
&& /.env/bin/pip install --no-cache-dir -r requirements.txt \
&& apk del .build-deps

VOLUME ["/capstone", "$PYTHON_ENV"]
WORKDIR /capstone
CMD ["tail", "-f", "/dev/null"]

