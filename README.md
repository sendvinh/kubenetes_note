# kubenetes_note

## Kubernetes Components

#### node
simple server, physical or virtual machine
#### pod
smallest unit of k8s  
abstraction over a container  
it creates running environment (layer on top of the container)  
(kubernetes wants to abstract away the container runtime)  
usually 1 application per pod  
each pod gets its own IP address  
new ip address on re-creation  
#### service
provides static/permanent ip address that can be attached to each pod  
lifecycle of Pod and Service NOT connected -> if Pod dies, service and its IP address still stay  
internal service  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - name: http
    port: 80
    targetPort: 8080
```
#### ingress
provide external access to Kubernetes services  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /foo
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              name: http
```
#### configMap
external configuration of your application  
dont put credentials into configMap
#### secret
similar to configmap, used to store secret data (in base64 encoded)
#### volumnes
storage on local node or remote (outside of k8s cluster)

#### deployment
manages a set of replicas of your application  
provide an easy way to manage the lifecycle of your application, including scaling, rolling updates, and self-healing in the event of failures  
Ex:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-webapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-webapp
  template:
    metadata:
      labels:
        app: my-webapp
    spec:
      containers:
        - name: my-webapp
          image: myregistry/my-webapp:1.0
          ports:
            - containerPort: 80
```
#### stateful set




