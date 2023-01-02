## Commands and Arguments in Docker
    docker run ubuntu
    docker ps
    docker ps -a
    
## ENV 
    1. Plain Key Value
        env:
          - name: APP_COLOR
            value: pink
    2. ConfigMap
        env:
          - name: APP_COLOR
            valueFrom:
                configMapKeyRef:
    3. Plain Key Value
        env:
          - name: APP_COLOR
            valueFrom:
                secretKeyRef:

### Create ConfigMaps
    1. Imperative
        kubectl create configmap \
            <config-name> --from-literal=<key>=<value>  \
                        --from-literal=<key>=<value>  \
                        --from-literal=APP_COLOR=blue 

        kubectl create configmap \
            <config-name> --from-file-=<>path-to-file

    2. Declarative
        kubectl create -f
            




