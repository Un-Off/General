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
        kubectl create -f config-map.yaml
        [config-map.yaml](./yamlFiles/config-map.yaml)

        app-config
            APP_COLOR: blue
            APP_MODE: prod
        
        mysql-config
            port: 3306
            max-allowed_packets: 128M
        redis-config
            port: 6379
            rdb-compression: yes

### ConfigMap in Pods
    
    1. ENV
        pod-definition.yaml
            envFrom:
              -  configMapRef:
                    name: app-config
    
    2. SINGLE ENV
        env:
          -  name: APP_COLOR
             valueFrom:
              configMapKeyRef:
                    name: app-config
                    key: APP_COLOR
    
    3. VOLUME
        volumes:
          -  name: app-config-volume
             configMap:
               name:  app-config