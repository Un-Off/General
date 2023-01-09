## create image
    create dockerfile name Dockerfile
     (provide all information like install packages, copy to, entrypoint)

    docker build Dockerfile -t <tag/name>
    docker push <userName/imageName> (to make available publicly on docker hub )

## Docker
    instruction argument

    FROM
    RUN
    COPY 
    ENTRYPOINT 

    docker history <image_name>

## Environment Variables
    docker run -e APP_COLOR=blue simple-webapp
    docker run -e APP_COLOR=green simple-webapp

## Commands vs Entrypoint
    CMD ["command","param1"]
    CMD ["sleep","5"]

    ENTRYPOINT ["sleep"]
    CMD ["5"]  (5 is default)

## Docker Compose
[dockerCompose](./yamlFiles/dockerCompose.yaml)

# Docker Engine
    Docker CLI
    REST API
    Docker Deamon

    docker -H=remote-docker-engine:2375
    docker -H=10.123.2..1:2375 run nginx

## File system
    Layered architecture
        each instruction create layer that is modified to the pervious

### volume mount
    docker volume create data_volume
    docker run -v data_volume:/var/lib/mysql mysql (/var/lib/docker)
    docker run -v data_volume2:/var/lib/mysql mysql (data_volume2 will be automatically created)
### bind mount
    docker run -v /data/mysql:/var/lib/mysql mysql
    docker run \
    --mount type=bind, source=/data/mysql, target=/var/lib/mysql mysql
## Storage driver
    AUFS
    ZFS
    BTRFS
    Device Mapper
    Overlay
    Overlay2

# Docker Networking
## Default networks
        Bridge (a network with a ip, bridged to containers)
            docker run ubuntu
        none (no network, container run isolate)
            docker run ubuntu --network=none
        host
            docker run ubuntu  --network=host

## User-defined networks
    docker network create \
        --driver bridge \
        --subnet 182.18.0.0/16
        custom-isolated-network

# Docker Registry
    docker login private-registry.io    

    docker run -d -p 5000:5000 --name registry registry:2
    docker image tag my-image localhost:5000/my-image
    docker push localhost:5000/my-image
    docker pull localhost:5000/my-image
    docker pull 192.168.56.100:5000/my-image

# Container Orchestration
    docker service create --replicas=100 nodejs


    






