# SteVe-Docker

## Introduction

SteVe-Docker is a mechanism of distributing the [SteVe](https://github.com/RWTH-i5-IDSG/steve) EVCMS via Docker and Docker Compose, with ancillary services, as follows:
* [MariaDB](https://github.com/MariaDB/server)
* [Flyway](https://github.com/flyway/flyway)
* [nginx](https://github.com/nginx/nginx)
* Irasus OCPP 1.6 JSON Client Simulator via nginx

## Architecture

### Ports

The following ports on the host system are mapped to a port in a container in the Docker engine:
* 80: nginx

The following ports are being used by the respective containers:
* 80: nginx reverse proxy
* 80: nginx Irasus OCPP 1.6 JSON Client Simulator
* 3306: MariaDB
* 8180: SteVe

## Instructions to use

* Create the Irasus SteVe Docker image if it does not exist already.

	```
	docker build -t irasus/steve:latest . -f Dockerfile.copy
	```

* Initialize SteVe's MariaDB database using Flyway

	```
	docker-compose -f SteVe__Setup_database.yml up
	```

* Destroy the containers created for initializing SteVe's MariaDB database using Flyway

	```
	docker-compose -f SteVe__Setup_database.yml down
	```

* Deploy SteVe and ancillaries

	```
	docker-compose -f SteVe__Run_app.yml up
	```

* Login to SteVe via [http://steve.localhost/](http://steve.localhost/) using the credentials ```admin```, ```1234``` and create a charger "CHARGER" and a tag "TAG"

* Open the simulator via [http://simulator.localhost/](http://simulator.localhost/) and validate the Central Station URL ```ws://steve.localhost/charger/CHARGER```. Click on "Connect to the EVCMS".

* Access SteVe's REST API via SteVe's Swagger webpage at [http://steve.localhost/swagger-ui.html](http://steve.localhost/swagger-ui.html)
