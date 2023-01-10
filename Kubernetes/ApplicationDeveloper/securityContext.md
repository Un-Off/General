## Security Context
 ```yaml
  (pod level)
  kind: Pod
  spec:
    securityContext:
      runAsUser: 1000

  (container level)
  kind: Pod
  spec:
    containers:
      -  securityContext:
          runAsUser: 1000
  ``` 
## Resource Requirements
 ```yaml
  kind: Pod
  spec:
    containers:
      - name : containername
        resources:
          requests:
            memory: "1Gi"
            cpu: 1
  ```
    CPU 0.1 = 100m (milli)
        1 = 1AWS vCPU = 1GUP Core= 1Azure Core = 1 Hyperthread
    Memory
        G Gigabyte, M Megabyte
        1Gi (Gibibyte), 1Mi (Mebibyte), 1Ki (kibibyte = 1024 bytes)

## Resource Requirements
 ```yaml
  kind: Pod
  spec:
    containers:
      - name : containername
        resources:
          requests:
            memory: "1Gi"
            cpu: 1
          limits:
            memory: "2Gi"
            cpu: 2
  ```