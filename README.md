# SteVe-Docker

## Introduction

SteVe-Docker is a mechanism of distributing the [SteVe](https://github.com/RWTH-i5-IDSG/steve) EVCMS via Docker and Docker Compose, with ancillary services, as follows:
* [Node-RED](https://github.com/node-red/node-red)
* [MariaDB](https://github.com/MariaDB/server)
* [Flyway](https://github.com/flyway/flyway)
* [nginx](https://github.com/nginx/nginx)
* Irasus OCPP 1.6 JSON Client Simulator via nginx

## Architecture

### Ports

The following ports on the host system are mapped to a port in a container in the Docker engine:
* 80: nginx
* 2200-2300: Node-RED
* 1880: Node-RED

The following ports are being used by the respective containers:
* 80: nginx reverse proxy
* 80: nginx Irasus OCPP 1.6 JSON Client Simulator
* 2200-2300: Node-RED
* 1880: Node-RED
* 3306: MariaDB
* 8180: SteVe

## Instructions to use

* Grant permissions to the Node-RED container to access the volumes for persistent data storage

	```
	chown -R $USER:$USER node-red/
	find node-red/ -type f -exec chmod 666 {} \;
	find node-red/ -type d -exec chmod 777 {} \;
	```

* Deploy SteVe and ancillaries

	```
	docker-compose up
	```

* Login to SteVe via [http://steve.localhost/](http://steve.localhost/) using the credentials ```admin```, ```1234```. Create a charger "CHARGER" and a tag "TAG". Enable webhook notifications in "SETTINGS" for all events, and specify the webhook target ```http://node-red:1880/```.

* Open the simulator via [http://simulator.localhost/](http://simulator.localhost/) and validate the Central Station URL ```ws://steve.localhost/charger/CHARGER```. Click on "Connect to the EVCMS".

* Access SteVe's REST API via SteVe's Swagger webpage at [http://steve.localhost/swagger-ui.html](http://steve.localhost/swagger-ui.html)

* Access SteVe's Webhooks via Node-RED at [http://localhost:1880/](http://localhost:1880/)
