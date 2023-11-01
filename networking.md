# networking 101


### Example of a cluster with one node

The node has a public IP address. (192.168.1.2)

The node has a single pod, and a pod hosts a container.

Each pod gets its own internal IP address. (10.244.0.2)

The subnet of the node is 10.244.0.0/24

A second pode would have the 10.244.0.3 address.

etc.

### cluster with multiple nodes

By default, the pods of each nodes could have the same subnet address space and probably end up having repeated internal IP addresses. This can cause problems.

Kubernetes expects the user to set up the networking specifications and solutions to meet the criteria:

- all containers/pods can communicate to one another without NAT

- all nodes can communicate with all containers and vice-versa without NAT

**prebuit solutions**

- Cisco ACI networks
- flannel
- vmware NSX 
- cilium
- calico

These solutions handle the routing, vnets, and subnet address spaces so that they are not repeated.
