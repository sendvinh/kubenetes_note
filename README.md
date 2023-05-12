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
provide load balancer
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
#### volumes
storage on local node or remote (outside of k8s cluster)

#### deployment
manages a set of replicas of your application  
the replica is connected to the same service  
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
db can't be replicated via deployment (because database have state - data storage) -> statefulSet  
StatefulSet is designed for managing stateful applications that require stable network identifiers (such as hostnames) and persistent storage.  
note: DB are often hosted outside of K8s cluster  

## Kubernetes Architecture  

### Node processes - Worker machine in K8s cluster  
3 processes must be installed on every worker node  

**Container runtime**  
is responsible for **running and managing containers** on a node in a Kubernetes cluster. It provides an environment for containers to run in, including isolation, networking, and storage  
**Kubelet**  
interacts with both - the container and node. Kubelet starts the pod with a container inside. Kubelet is an agent that runs on each node in a Kubernetes cluster. It **communicates with the Kubernetes API server** to receive information about which pods should be running on the node, and then **manages the containers and other resources associated with those pods**.  
**Kubeproxy**   
forwards the requests. It is responsible for **managing network routing**. It runs on each node in the cluster and maintains network rules to allow communication between different pods and services within the cluster.  
--> how to interact with cluster? (schedule pod, monitor, restart pod, reschedule, join a new node ...) -> Master Node  

### Master processes  
4 processes run on every master node  

**Api Server**  
is like cluster gateway, which gets the initial request of any updates into the cluster  
acts as a gatekeeper for authentication for authentication  
It serves as the front-end for the Kubernetes control plane, providing a REST API for all Kubernetes resources  
request -> api server -> validates request -> other processes -> pod  

**Scheduler**  
responsible for scheduling workloads onto the available nodes in the cluster  
Schedule new Pod -> api server -> Scheduler -> where to put the pod?  -> kubelet will execute  
Scheduler just decides on which Node new Pod should be scheduled  

**Controller manager**  
watch the state of the cluster and make changes to maintain the desired state  

**etcd**  
it's cluster brain
cluster changes get stored in the key value store in etcd  
-> that's why how scheduler know what resources are available; how it know cluster state change...  
note: application data is NOT stored in etcd!

**KUBENETES cluster is usually made of multiple Master nodes**  
**Master nodes usually need less resources than worker nodes**

## Minikube and kubectl  
...  
## Main Kubectl Commands  
...  
## Helm && Helm charts  
Helm is package manager for Kubernetes  
Helm charts  
- is bundle of yaml files  
- create your own Helm Charts with Helm  
- Push them to Helm Repository
- Download and use existing ones  
helm charts in public registries and private registries  
#### Templatting Engine
Template yaml config (e.g {{.Values.name }})  
get from values.yaml file  
directory structure  
mychart/  
 |--Chart.yaml  
 |--values.yaml  
 |--charts/  
 |--templates/  
 













