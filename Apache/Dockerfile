FROM httpd:2.4

ENV APACHE_SOCK /var/run/apache2 
ADD ./requirements.txt ./
RUN apt-get update \
&& apt-get install -y \
python3 python3-venv python3-pip libapache2-mod-wsgi-py3 \
&& python3 -m venv /.env \
&& /.env/bin/pip install --no-cache-dir -r requirements.txt \
&& rm -rf /var/lib/apt/lists/* \
&& mkdir -p $APACHE_SOCK  
