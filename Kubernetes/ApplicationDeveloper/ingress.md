# Ingress

## Ingress Controller

    Deployment
    Service
    ConfigMap
    Auth

    ```yaml
    # Deployment
    apiVersion: apps/v1
    kind: Deployment
    metadata: 
      name: nginx-ingress-controller
    spec:
      replicas: 3
      selector: 
        matchLabels:
          name: nginx-ingress
      template:
        metadata:
          labels:
            name: nginx-ingress
          spec: 
            containers:
              - name: nginx-ingress-controller
                image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
            args:
              - /nginx-ingress-controller
              - --configmap=$(POD_NAMESPACE)/nginx-ingress-configuration
            env:
              - name: POD_NAME
                valueForm:
                  fieldRef:
                    fieldPath: metadata.name
              - name: POD_NAMESPACE
                valueForm:
                  fieldRef:
                    fieldPath: metadata.namespace
            ports:
              - name: http
                containerPort: 80
              - name: https
                containerPort: 443
            
    ```

    ```yaml
    # service
    apiVersion: v1
    kind: Service
    metadata: 
      name: nginx-ingress
    spec:
      type: NodePort
      ports:
        - port: 80
          targetPort: 80
          protocol: TCP
          name: http
        - port: 443
          targetPort: 443
          protocol: TCP
          name: https
      selector: 
        name: nginx-ingress
    ```

    ```yaml
    # ConfigMap
    Kind: ConfigMap
    apiVersion: v1
    metadata:
      name: nginx-configuration
    ```

    ```yaml
    # Service account
    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: ingress-serviceaccount


    # should contain right Roles, ClusterRoles and RoleBindings
    ```

## Ingress Resource

    ```yaml
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: ingress-wear
    spec:
      backend:
        serviceName: wear-service
        servicePort: 80
    ```
    k create -F ingress-wear.yaml
    k get ingress

    ```yaml
    # multiple path
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: ingress-wear-watch
    spec:
      rules:
        - http:
            paths:
              - path:  /wear
                backend:
                  serviceName: wear-service
                  servicePort: 80
              - path:  /watch
                backend:
                  serviceName: watch-service
                  servicePort: 80
    ```

    ```yaml
    # multiple url
    apiVersion: extensions/v1beta1
    kind: Ingress
    metadata:
      name: ingress-wear-watch
    spec:
      rules:
        - host: wear.my-store.com 
          http:
            paths:
              - backend:
                  serviceName: wear-service
                  servicePort: 80
        - host: watch.my-store.com 
          http:
            paths:          
              - backend:
                  serviceName: watch-service
                  servicePort: 80
    ```
    multiple path in different multiple url is possible by adding path under paths
