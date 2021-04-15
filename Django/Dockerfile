FROM debian:buster-slim

ENV PYTHON_ENV=.env

ADD requirements.txt ./
RUN apt update \
&& apt install -y \
gcc \
musl-dev \
python3-venv \
python3-pip \
python3-dev \
&& python3 -m venv /$PYTHON_ENV \
&& /.env/bin/pip install -v --no-cache-dir -r requirements.txt \
&& rm -rf /var/cache/apt/archives/

VOLUME ["/capstone", "$PYTHON_ENV"]
WORKDIR /capstone

