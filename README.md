# Django with Apache and PostgreSQL in a separate container

This a capstone project at our school for our final year, at first I didn't apply docker because we want it to be simple and not complicating it for our final year project. After months of studying docker and docker compose, I have finally dockerized my capstone project, well I mean most of it, regular http is not working at the moment and only support https protocol.

![idea](./images/Capstone_Project_Dockerized.png)

## Create network 

```
docker create network -d bridge app
```


## Build and run manually
* Build and run the django image

```
docker build --no-cache -t django:1.0 .
docker run --name django -h django -d --net vpc -v $(pwd)/Django/Capstone_16ES:/capstone django:1.0 ./run.sh 
```

* Run the postgres image

```
docker run --name postgres --net vpc -h postgres -p 5432:5432 -e POSTGRES_USER=<user> \ -e POSTGRES_PASSWORD=<password> -e POSTGRES_DB=db -d
```

* Building and running Apache image

```
docker build --no-cache apache:1.0 .
docker run --name apache --volumes-from django \ 
-v $(pwd)/Apache/httpd-config/my-httpd.conf:/usr/local/apache2/conf/httpd-conf \
-v $(pwd)/Apache/httpd-config/capstone.conf:/usr/local/apache2/conf/extra/httpd-vhosts.conf \
-v $(pwd)/Apache/httpd-config/my-httpd-ssl.conf:/usr/local/apache2/conf/extra/httpd-ssl.conf \
-v $(pwd)/cert:/usr/local/apache2/conf/ \
-v $(pwd)/mime.types:/usr/local/apache2/conf/mime.types --net vpc -p 80:80 -p 443:443 apache:1.0 httpd -D FOREGROUND
```

## Build and run with a composer
* Building and running with docker compose 

```
docker-compose up
```






