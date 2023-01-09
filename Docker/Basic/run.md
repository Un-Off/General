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
    