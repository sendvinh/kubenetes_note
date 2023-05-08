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
static/permanent ip address that can be attached to each pod 
lifecycle of Pod and Service NOT connected -> if Pod dies, service and its IP address still stay
#### node
#### node
