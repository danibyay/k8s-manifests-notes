# Pod related kubectl commands

when creating an object both work

        kubectl apply
        kubectl create

when applying changes to existing objects, only apply will work

example

        kubectl apply -f pod.yaml
        kubectl get pods
        kubectl describe pod nginx 
        # nginx is the name of the pod

you can create a pod without a yaml file

        kubectl run nginx --image=nginx

You can create a pod without writing a yaml file yourself but still create a yaml file in the process

        # gives the output of the file
        kubectl run redis --image=redis --dry-run=client -o yaml

That would give you the contents of a file as output and you would have to copy paste, but what about using the linux > operator to pass the outputs into a file at the same time? 

        kubectl run redis --image=redis --dry-run=client -o yaml > redis.yaml

delete the pod

        kubectl delete pod webapp
        # webapp is the name of the pod

Get more info about all pods, instenad of running describe on each one. For example on which node they're running

        kubectl get pods -o wide