# Deployments

### Rolling update

Updating pods one at a time to not impact the availability for the users.

Instead of all at once and shutting the service down completely

A pod is a single instance of our application. 

A replica set is a group of pods with auto scaling capabilities.

A deployment is a kubernetes object with higher hierarchy than the replica set. 

The capabilities of the deployment include:

- rolling updates 
- being able to roll back to a previous version
- pause & resume changes

The deployment automatically creates a replicaset

And the replicaset automatically creates pods

### commands

        kubectl create -f deployment-definition.yaml
        kubectl get deployments
        kubectl get all # to see all objects
        kubectl describe deployment myapp-deployment

## Rollout and versionning

When you first create a deployment, it triggers a rollout, and a new rollout creates a new revision.

        kubectl rollout status deployment myappdeployment
        kubectl rollout history deployment myappdeployment

### Deployment strategy

**Recreate**

destroy current pods, and create new ones with newer app version.

Disadvantage: there will be a time where the app is down

        kubectl delete deployment x
        kubectl create -f deployment.yaml 

Use the record option at the end, so that in the rollout history command this command will appear as the change-cause of the new revision created

        kubectl create -f deployment.yaml --record

**Rolling update**

It's the default strategy. The application never goes down.Upgrade is seamless

One instance is killed, one is updated, one by one this goes on until it's finished. 

This strategy uses two replica sets to do the upgrade, and at the end it deletes the old one.

Alternatives to do it:

1. Edit the yaml definition file and run 

        kubectl apply -f deployment-definition.yaml

2. Run a command to update the image version of the containers of the deployment: 

pattern is container_name=image:tags

        kubectl set image deployment/myapp-deployment nginx-container=nginx:1.9.1 --record

3. Run a command that opens the editor of the yaml file, it has the same problem as option 2 because this file is temporary, but the changes you can do are more flexible.

        kubectl edit deployment myappdeployment --record

Choosing option 2, is leaving your definition file outdated and causing confusion. The file will not update automatically if you choose to use the cli.

How many pods can be deleted at a time, is controlled by the attribute of the strategy maxUnavailable. and it is stated as a percentage of the total pods. (25% of a replicaset of 4 would be only one at a time)


### Rollback

        kubectl rollout undo deployment-myappdeployment

When you run an undo, the number of revisions will not change, because essentially, what you are deploying is not new, it's just different from the most recent revision, but you already had it before

example:

    revision
    1     kubectl create
    2     update nginx to nginx 1.1.0
    3     update nginx to nginx 1.2.0

After an undo, revision 4 is the same as what we had in revision 2, and revision 2 is gone.

    revision
    1     kubectl create
    3     update nginx to nginx 1.2.0
    4     update nginx to nginx 1.1.0


### What happens when you try to update the deployment with errors?


The application is not impacted, because the new replica set is in error state and the old replicaset is not killed, it's still working.

Kubernetes doesn't kill old pods until the new ones are available.

