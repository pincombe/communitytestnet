# Community Test Net -- SINGLE NODE

These are instructions for a single node

The docker image can be built from the Dockerfile, however it is advised to pull it from docker hub. Docker was chosen as a lightweight option to execute factomd within an enviroment the Factom developers can have access to, without having access to the host machine.

## Docker image

The docker image is constructed to allw for factom developers to have ssh access to the docker container for remote debugging and analyzing. This does not grant ssh access to the host machine, only the docker container.

## Run with Docker

First copy the config file, and if you are an authority node, you will have to edit it.
```
cp factomd.conf.EXAMPLE factomd.conf
```


Create the volume for database persistance, then run the docker image created for the testnet

```
echo "Creating persistant docker volume"
docker volume create factomd_volume

echo "Pushing config for factomd with custom identity (if authority node)"
docker run --rm -v ${PWD}/factomd.conf:/source -v factomd_volume:/destination busybox /bin/cp /source /destination/factomd.conf

docker run -d -p 8109:22 -p 8090:8090 -p 9876:9876 -p 8110:8110 -v factomd_volume:/root/.factom/m2 --name factomd_testnet_community_node emyrk/factomd_testnet_community:v1
```

## Ports to Open

Various ports will need to be opened (port forwarded) to allow for outside connections coming inbound.

* 8110 -- Port used for Peer to Peer networking
* 8220 -- Port used for Factom Dev ssh

Other ports you may be interested in

* 9876 -- Prometheus Metrics
* 8090 -- Control Panel

## Build from source

Copy the factomd directory into this one, then build

```
cp $GOPATH/src/github.com/FactomProject/factomd .
docker build -t factomd_testnet_community .
```