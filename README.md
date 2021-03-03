# Django with Apache and PostgreSQL
## Build from a composer
## Build manually
* Build and run the django image

```
docker build --no-cache -t django:1.0
```

* Run the postgres image

```
docker run --name postgres --net app -h postgres -p 5432:5432 -e POSTGRES_USER=john \ -e POSTGRES_PASSWORD=123 -e POSTGRES_DB=db -d
```

* Building and running Apache image



