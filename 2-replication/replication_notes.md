# Replication

Replication controller is the old technology, now replaced by replica set.

Replica set is the recommended resource to use when you need replication.

Replication provides high availability and load balancing in the kubernetes cluster.

## replication controller

A replication controller creates multiple instances of a pod, or if you only have one pod, it will be in charge of recreating it when/if it fails.

When creating a replication controller, under the spec section of the yaml, you will essentially write the contents of what a pod yaml file would have.

The replication controller is the parent, and the pod is the child.
(two definition files together)

To create the rc, write the command

        kubectl create -f rc-definition.yaml

To view them

        kubectl get replicationcontroller

The pods created by the rc will have the prefix of the rc name on their name

## replica set

The definition file looks similar, plus the selector argument under spec. Used to specify which pods fall under it.

why? 

Replica set can also manage pods that were not created as part of the replica set!

The replication controller has the **selector** attribute available, but it's not mandatory.

The selector works through labels.

        kubectl create -f replicaset-definition.yaml
        kubectl get replicaset
        kubectl get rs
        kubectl describe replicaset myapp-replicaset
        kubectl get pods
        kubectl delete replicaset myapp-replicaset

### How to scale it ?

How to change the number of desired running pods ?

a) Just update the field of number of replicas and run `kubectl replace -f replicaset-definition.yaml`

b) Run `kubectl scale --replicas=6 -f replicaset-definition.yaml`

c) Run `kubectl scale --replicas=6 replicaset myapp-replicaset` where you specify the name of the replicaset as the last argument

d) Edit a temporary file by opening it with the edit command `kubectl edit replicaset myapp`. The changes will be applied as soon as the file is saved. No need to run apply command.

### important

The replicaset not only creates new pods if necessary, but it will also delete new pods if they are more than what we said we needed.