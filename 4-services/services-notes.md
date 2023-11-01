# services

Services enable communication between components of the application, and help connect applications with other apps or users.

Services enable loose coupling between microservices.

A service is an object just like pods, replicasets or deployments.

## NodePort

### use case

how to map requests from the outside of the cluster into the apps running inside a node?

**NodePort service** 

listens to a port on the node and forwards requests to the pods

Other kinds of services

### More about the NodePort

Must be in the range of 30,000 - 32,767

The mandatory field is port.

You can have multiple ports mapping with a single service. (in the yaml, ports is an array)

### Multiple pods

If you have multiple pods of the same app for high availability.

The selector label will match all of these pods, all these pods will be enpoints for the requests coming through the port. (expected behaviour)

The algorithm used for load balancing is random

### Multiple nodes

If the pods with the same label are distributed across multiple nodes, the services will automatically map the the requests across the nodes.

### commands

        kubectl create -f service-definition.yaml
        kubectl get services
        kubectl get svc


## cluster IP

virtual IP inside the cluster to enable communication

At launch, Kubernetes  has a clusterIP service running already.

## Load Balancer

When there are multiple nodes and pods for the same instance and they have each their IP address, the user wants a single url to enter the application.

Kubernetes integrates with the native LB of a cloud provider.

Better than nodeport

Supported by GCP, Azure, and AWS.


## Endpoints

A service has as many endpoints as pods associated with the selector tags