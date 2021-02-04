
# Running Casasandra in docker containers

Run two Cassandra docker containers with different names, here they are cassandra and cassandra2
The schema scripts can be found [here](https://github.com/jaegertracing/jaeger/tree/master/plugin/storage/cassandra/schema). 
Creating Cassandra backend storage before starting these services:

    1.  sudo docker network create  socialnetwork_default
    2.  sudo docker run -p 9042:9042 --name cassandra --network socialnetwork_default \
        -v /path/to/data/storage:/var/lib/cassandra \
        -v /path/to/init/scripts:/docker-entrypoint-initdb.d \
        -e MODE=test -d cassandra:latest
    3.  sudo docker exec -ti cassandra bash and then type nodetool status
    4.  sudo docker run --name cassandra2 --network socialnetwork_default \
        -e CASSANDRA_SEEDS="cassandra-1 server" -d cassandra:latest
    5.  sudo docker exec -ti cassandra bash 
        - cd docker-entrypoint-initdb.d/
        - ./create.sh | cqlsh
    6.  finally change the line 258 in docker-compose.yml

# Build the docker image from source code:

1. From socialnetwork/, build the socialnetwork image:
    ``` sudo docker build -t socialnetwork .```
2. Use docker-compose to run all the different services:
    ``` sudo docker-compose up -d```