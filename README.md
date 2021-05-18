# Django in a LAMP stack in seperate container

This a capstone project at our school for our final year, at first I didn't apply docker because we want it to be simple and not complicating it for our final year project. After months of studying docker and docker compose, I have finally dockerized my capstone project, well I mean most of it, regular http is not working at the moment and only support https protocol.

![idea](./images/Capstone_Project_Dockerized.png)

## Dependencies
* Docker

> You can get the latest Docker Version from the Docker Official Website 
> For Ubuntu based [here](https://docs.docker.com/engine/install/ubuntu/)
> For Debian based [here](https://docs.docker.com/engine/install/debian/)
> For Red Hat/CentOS [here](https://docs.docker.com/engine/install/centos/)
* Docker Compose

> For Debian based use this commmand

```
sudo apt install docker-compose
```

> For RedHat Based machine use this command

```
sudo yum install docker-compose
```

## Create network 

```
docker create network -d bridge <network-name>
```

## Environment variable

> The PostgreSQL image uses several environment variables which are easy to miss. The only variable required is ```POSTGRES_PASSWORD```, the rest are optional. 
> The following can be change to suit your project 

```
POSTGRES_USER: user_here
POSTGRES_PASSWORD: password_here
POSTGRES_DB: databasename_here
```

## Compose Option

> These option are very important because instead of hardcode the compose file, these option can help us to shorten the process of creating and connecting container together

```
hostname
depends_on
```

## Build and run manually
* Build and run the django image

```
docker build --no-cache -t Django/django:1.0 .
docker run --name django -h django -d --net <network-name> -v $(pwd)/Django/Capstone_16ES:/capstone django:1.0 ./run.sh 
```

* Run the postgres image

```
docker run --name postgres --net <network-name> -h postgres -p 5432:5432 -e POSTGRES_USER=<user> \ -e POSTGRES_PASSWORD=<password> -e POSTGRES_DB=db -d
```

* Building and running Apache image

```
docker build --no-cache -t Apache/apache:1.0 .
docker run --name apache -h apache --volumes-from django \ 
-v $(pwd)/Apache/httpd-config/my-httpd.conf:/usr/local/apache2/conf/httpd-conf \
-v $(pwd)/Apache/httpd-config/capstone.conf:/usr/local/apache2/conf/extra/httpd-vhosts.conf \
-v $(pwd)/Apache/httpd-config/my-httpd-ssl.conf:/usr/local/apache2/conf/extra/httpd-ssl.conf \
-v $(pwd)/cert:/usr/local/apache2/conf/ \
-v $(pwd)/mime.types:/usr/local/apache2/conf/mime.types --net <network-name> -p 80:80 -p 443:443 apache:1.0 httpd -D FOREGROUND
```

## Build and run with a composer
* Building and running with docker compose 

```
docker-compose up
```

## Run with Docker Swarm
* Generate swarm token 

```docker
docker swarm init
```

* Copy the token generated and run 

```
docker swarm join --token <generated-token> <manager-ip>:<port-generated>
```

* Create an overlay network for swarm to work
The name corresponding to the name of the network in the compose file 

```
docker network create -d overlay <network-name>
```

* Deploy with a docker compose file

```
docker stack deploy -c <compose-name> <name>
```

* **Optional** Scale the service

```
docker service scale <service-name>=<number-of-task>
```

## Securing Docker daemon with TLS and remote access to Docker Engine (Optional)

> To directly control the Docker Engine inside remote VM or cloud servers without having to resolve with SSH connection. Even tho SSH connection is practically secure with private key from the client side and public key on the server side but the SSH protocol uses port 22 which is a well-known port that many outsiders already knew and may attempt to access it and login to the VM. Instead, if the VM is only running docker then the client side doesn't need to access and control the Docker Engine via SSH connection (the client side could if they want to). 

* Create a Certificate Authority

```
echo 01 | sudo tee ca.srl
sudo openssl genrsa -des3 -out ca-key.pem
sudo openssl req -new -x509 -days 365 -key ca-key.pem -out ca.pem
```

* Create a server certificate signing request and key

> Generating a private key
```
sudo openssl genrsa -des3 -out server-key.pem
```

> Creating a CA certificate

```
sudo openssl req -new -x509 -days 365 -key ca-key.pem -out ca.pem
```

* Create a server certificate signing request and key

> Creating a server key

```
sudo openssl genrsa -des3 -out server-key.pem
```
> Creating our server CSR

```
sudo openssl req -new -key server-key.pem -out server.csr
```

> Connect via IP address
> Replacing x.x.x.x with the IP address(es) of your Docker daemon

```
echo subjectAltName = IP:x.x.x.x,IP:127.0.0.1 > extfile.cnf
```

> Signing our CSR

```
sudo openssl x509 -req -days 365 -in server.csr -CA ca.pem \
-CAkey ca-key.pem -out server-cert.pem -extfile extfile.cnf
```

> Removing the passphrase from the server key

```
sudo openssl rsa -in server-key.pem -out server-key.pem
```

> Securing the key and certificate on the Docker server

```
sudo chmod 0600 server-key.pem server-cert.pem ca.pem
```

* Configuring the Docker daemon

> Enabling Docker TLS on systemd (Optional)

```
ExecStart=/usr/bin/docker -d -H tcp://<VM_IP_Address>:<Random_Port> --tlsverify --
tlscacert=<ca.pem File_location> --tlscert=<server-cert.pem File_location> --tlskey=<server-key.pem File_location>
```

> Reloading and restarting the Docker daemon

```
sudo systemctl --system daemon-reload
```
> Using Docker Daemon from config file (Optional)

```
{
	"debug" : true,
	"hosts" : ["tcp://<VM_IP_Address>:<Random_Port>", "unix:///var/run/docker.sock"],
	"experimental" : true,
	"log-driver" : "json-file",
	"log-opts" : {
		"max-size" : "20m",
		"max-file" : "3",
		"labels" : "develope_status",
		"env" : "developing"
	}
}
```

> Start the Dockerd daemon

```
sudo dockerd --selinux-enabled
```

* Creating a client certificate and key

> Creating a client key

```
sudo openssl genrsa -des3 -out client-key.pem
```

> Creating a client CSR

```
sudo openssl req -new -key client-key.pem -out client.csr
```

> Adding Client Authentication attributes

```
echo extendedKeyUsage = clientAuth > extfile.cnf
```

> Signing our client CSR

```
sudo openssl x509 -req -days 365 -in client.csr -CA ca.pem \
-CAkey ca-key.pem -out client-cert.pem -extfile extfile.cnf
```

> Stripping out the client key pass phrase

```
sudo openssl rsa -in client-key.pem -out client-key.pem
```

* Configuring our Docker client for authentication

> Copying the key and certificate on the Docker client

```
mkdir -p ~/.docker/ \
&& cp ca.pem ~/.docker/ca.pem \
&& cp client-key.pem ~/.docker/key.pem \
&& cp client-cert.pem ~/.docker/cert.pem \
&& chmod 0600 ~/.docker/key.pem ~/.docker/cert.pem
```

* Testing TLS-authenticated connection

> With ```--tslverify``` argument to connect with TLS

```
sudo docker -H=<VM_IP_Address>:<Random_Port> --tlsverify info
```

> Without ```--tslverify``` argument it would return 

```
Error response from daemon: Client sent an HTTP request to an HTTPS server.
```

> Which mean that everything worked 



## Restricton
> This swarm can only scale the Django app and Web App, not the database, because of stability. When Django trying to connect to the database with its IP address, Django assume there is only one database with one IP address. But when we scale the database in the swarm, docker swarm create more database task, which mean that there are two database services with the same IP address, those databases conflict with eachother causing Django to panic and gives out error 







