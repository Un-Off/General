## Basic Docker Commands

    docker ps (list container, -a running and exited container)
    docker stop <container_name>
    docker rm <container_name> (to remove stopped or exited container)

    docker images (list imaged)
    docker rmi nginx (make so no container running with image to be removed)
    docker pull nginx

    docker exec distracted_mcclintock cat /etc/hosts
    docker run kodekloud/simple-webapp (will run in foreground. can't use the terminal till this process ends)
    docker run -d kodekloud/simple-webapp (detach mode. let the terminal free, run in the background)
    docker attach <containerId>

    docker run image:version

## STDIN

    docker run <image>
    docker run -it <image> (i interactive ask stdin, t sudo terminal prints prompt messages)

## PORT mapping

    docker run -p 80:5000 <image>
    docker run -p 8000:5000 <image>
    docker run -p 8001:5000 <image>

## Volume mapping

    docker run -v /opt/datadir:/var/lib/mysql mysql
    (/opt/datadir is external volume mounting to container /var/lib/mysql.when container is destroyed that data persist in the external volume )

## Inspect container

    docker inspect <container_name>
    (detailed container info)

## Container Logs

    docker log <container_name>
