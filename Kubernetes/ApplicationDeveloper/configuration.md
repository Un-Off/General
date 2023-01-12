## Service Account
    k create serviceaccount dashboard-sa
    k get serviceaccount
    k describe serviceaccount dashboard-sa
    k create token <serviceaccount_name>
    k describe secret <token>
    
    k exec -it <pod-name> ls <mount_path>
    
    deployment spec: --> template: --> spec: --> serviceAccountName: <serviceaccount_name>

## TAINTS AND TOLERATIONS
    taints on node 
    toleration on pods

    taint-effect (NoSchedule, PreferNoSchedule, NoExecute)

    k taint nodes node-name key=value:taint-effect
    ex: k taint nodes node1 app=blue:NoSchedule

    pod-def.yaml
```yaml
    spec:
      tolerations:
        -  key: "app"
           operator: "Equal"
           value: "blue"
           effect: "NoSchedule"
```

    pods with toleration are not restricted to be scheduled on nodes without taints
    node with taints restrict pods without tolerations

    facts: master node is created with taint: "NoSchedule". hence scheduler can't schedule pods in master node

## Node Selectors Logging

    pod-def.yaml
```yaml
    spec:
      nodeSelector:
        size: Large
```
    
    k label nodes <node-name> <label-key>=<label-value>
    k label nodes node-1 size=Large

## Node Affinity
    pod-def.yaml
```yaml
    spec:
      affinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
            - matchExpression:
                - key: size
                  operator: NotIn
                  values:
                    - Small
```
```yaml
  operator: In
  values:
    - Large
    - Medium
```
```yaml
  operator: Exists
```
    requiredDuringSchedulingIgnoredDuringExecution:
    preferredDuringSchedulingIgnoredDuringExecution:

    requiredDuringSchedulingRequiredDuringExecution:

## multi container
## Readiness Probes
  pod status (Pending, ContainerCreating, Running)
  pod conditions (PodScheduled, Initialized, ContainersReady, Ready)[truce, false]

  1. HTTP Teat - /api/Ready
    pod : spec--> containers--> redinessProbs
    ```yaml
    readinessProbe:
      httpGet:
        path: /api/ready
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 8
    ```
  2. TCP Test - 3306
    ```yaml
    readinessProbe:
      tcpSocket:
        port: 3306
    ```
  3. Exec Command
    ```yaml
    readinessProbe:
      exec:
        command:
          -  cat
          -  /app/is_ready
    ```
## Liveness Probes
    change readinessProbe to livenessProbe.


## Logging    
    docker run kodekloud/event-simulator
    docker run -d kodekloud/event-simulator
    docker logs -f ecf (-f allows to  see live logs trail as the events are running in background)

    k create -f event-simulator.yaml
    kubectl logs -f <pod-name> <container-name>(container name is important if there are multiple containers running in a pod )

## Monitor
    3rd party
      METRICS SERVER, Prometheus, Elastic Stack, DATADOG, dynatrace
    
    Metrics Server (slimed down version of heapster)

    minikube addons enable metrics-Server (for minikube , for other clone them from git)
    git clone https://github.com/kubernetes-incubator/metrics-server.git

    kubectl create -f deploy/1.8+/

    kubectl top Node
    kubectl top pod

# POD Design
## Labels, Selectors & Annotations

  k httpGet pods --selector app=app1

  metadata --> annotations
  annotations:
    buildversion: 1.34

  given example
  ```yaml
  apiVersion: apps/v1
  kind: ReplicaSet
  metadata:
     name: replicaset-1
  spec:
     replicas: 2
     selector:
        matchLabels:
          tier: front-end
     template:
       metadata:
         labels:
          tier: front-end
       spec:
         containers:
         - name: nginx
           image: nginx
  ```

## Rolling Updates & Rollbacks
    k rollout status deployment/myapp-deployment
    k rollout history deployment/myapp-deployment  (--version=1)

    make the required changes in deployment-def.yaml file 
    k apply -f deployment-def.yaml
    (or)
    k set image deployment/myapp-deployment \
                          nginx=nginx:1.9.1       (--record)
    (above command will result in different config in the yaml file. it just update in the deployment not in the yaml file)

    StrategyType: Recreate
    StrategyType: RollingUpdate

    k rollout undo deployment/myapp-deployment


    kubectl rollout history deployment nginx --revision=1
    kubectl rollout undo deployment nginx --to-revision=1
    (With --to-revision=1, it will be rolled back with the first image we used to create a deployment as we can see in the rollout history output)

    