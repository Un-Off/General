# Basic

## Containers

 has its own Processes, Network and Mounts

 Virtual Machine : Hardware->OS->Hypervisor->Virtual Machine[OS->{Libes,Deps}->Application]
 containers  : Hardware->OS->Docker->Containers[{Libes,Deps}->Application]

 Docker Image : {package pr template or plan} whoes instances are Docker Containers
     In other words Multiple containers may run under same instance of a Image

### Container Orchestration

 Orchestrate multiple containers together (a layer above containers)
 (Docker Swarm, kubernetes, Handle multiple services
 Mesos)      Adv to scale up and down services based on the load
        all done automatically deployed and managed

Nodes(worker machine) or Minions - servers
Cluster group of Nodes
Master responsible for Orchestration of worker nodes in a Cluster

### Components

 API Server, etcd, Sheduler, Controller(brain), Container Runtime(software like docker), kubelet

 Master - {kube-apiserver, etcd, controller, scheduler}
 Worker Node - {Kubelet, Container Runtime}

### Kubectl

 run hello-minikube
 cluster-info
 get nodes   -o wide
 version --short

 Kubectl run nginx --image nginx  <!--?(creates a POD and drops a instance of nginx image. This image will be from the docker public hub by default. It can be set to get from private repo )-->
 Kubectl describe pod nginx

 kubectl create deployment nginx --image=nginx  <!--?(To create a deployment using an imperative command, use kubectl create)-->
 kubectl set image deployment nginx nginx=nginx:1.9.1

## PODs

 a single instance of an application.
 containers are encapsulated by PODs
 smallest object in kubernetes

 multiple PODs are added while scaling up
 multiple conatiners can be in a same node (while a container acts as helper conatiner). shares same network and storage space

 <!--* If you are not given a pod definition file, you may extract the definition to a file using the below command: -->
 kubectl get pod < pod-name > -o yaml > pod-definition.yaml
 <!--* Then edit the file to make the necessary changes, delete and re-create the pod. -->
 kubectl edit pod < pod-name >

 kubectl replace --force -f < pod-yam-file >

### YAML in Kubernetes

[pod-definition](./ymlFiles/pod-definition.yml)

## Replication

### Replicationcontroller

[rc-definition](./ymlFiles/rc-definition.yml)

 <!--* commands -->
 kubectl create -f rc-definition.yml
 kubectl get replicationcontroller

### ReplicaSets

[replicaset-definition](./ymlFiles/replicaset-definition.yml)

 <!--* commands -->

 kubectl create -f replicaset-definition.yml
 kubectl get replicaset
 kubectl delete rs myapp-replicaset
 or
 kubectl delete replicaset myapp-replicaset  <!--? also deletes all underlying PODs -->

 kubectl replace -f replicaset-definition.yml
 or
 kubectl scale --replicas=6 -f replicaset-definition.yml
 or
 kubectl scale --replicas=6 replicaset myapp-replicaset <!--? (preferred) -->

## Deployment

[deployment-definition](./ymlFiles/deployment-definition.yml)
 <!--* commands -->

 kubectl create -f deployment-definition.yml

 kubectl get deployments
 kubectl get replicaset
 kubectl get pods

 or

 kubectl get all

### Rollout and Versioning

 kubectl rollout status deployment/myapp-deployemnt
 kubectl rollout history deployment/myapp-deployemnt

### Update and Rollback

 Recreate
 Rolling Update (deafult)

 kubectl apply -f deployment-definition.yml
 or
 kubectl set image deployment/myapp-deployment conatiner-name=new-image-name

 kubectl rollout undo deployment/myapp-deployment

## Services

 svc

### NodePort

 TargetPort
 Port
 NodePort
[service-definition.yml](./ymlFiles/service-definition.yml)

 1. kubectl create -f service-definition.yml
 2. kubectl get services
 3. curl htttp://192.168.1.2:30008

 minikube service myapp-service --url

### ClusterIP

[service-definition.yml](./ymlFiles/service-definition.yml)
 change only the type : ClusterIP (it's the deafult)

### LoadBalancer

 Adviced to use native applications

## Micro Services

    docker run -d --name=redis redis
    docker run -d --name=db postgres:9.5
    docker run -d --namae=vote -p 5000:80 --link redis:redis voting-app
    docker run -d --namae=result -p 5001:80 --link db:db result-app
    docker run -d --link redis:redis --link db:db --namae=worker worker


    deploy containers
    enable connectivity
    external access

    1. Deploy PODs
    2. create services (ClusterIP)
      1. redis
      2. db
    3. create service (NodePort)
      1. voting-app
      2. result-app

    vcs
    pcs red hat cluster
    SLA service level aggriment
    tat turn around time
    down time

    cluster - heart beat -
    fensing
    bonding
    ethtool
    LACP

## NameSpaces

 kubectl get pods --namespaces=dev
 <!--* to swotch namespace -->
 kubectl config set-context $(kubectl config current-context) --namespace=dev

 kubectl create -f computer-quota.yaml
