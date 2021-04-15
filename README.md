# Django with Apache and PostgreSQL in a separate container

This a capstone project at our school for our final year, at first I didn't apply docker because we want it to be simple and not complicating it for our final year project. After months of studying docker and docker compose, I have finally dockerized my capstone project, well I mean most of it, regular http is not working at the moment and only support https protocol.

![idea](./images/Capstone_Project_Dockerized.png)

## Build manually
* Build and run the django image

```
docker build --no-cache -t django:1.0 .
```

* Run the postgres image

```
docker run --name postgres --net app -h postgres -p 5432:5432 -e POSTGRES_USER=<user> \ -e POSTGRES_PASSWORD=<password> -e POSTGRES_DB=db -d
```

* Building and running Apache image

```
docker build --no-cache apache:1.0 .
```

## With a composer
* Building and running with docker compose 

```
docker-compose up
```






