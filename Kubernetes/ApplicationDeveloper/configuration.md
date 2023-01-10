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
