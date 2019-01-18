In ours earlier Pods lab, we

To run a Kubernetes deployment from the command-line:

`kubectl run nginx --image=nginx --port 80`{{execute}}

You just deployed your first application by creating a deployment.

This command automatically performed a few things for you:

* it searched for a suitable node where to run the application (although we have only 1 available node)
* it scheduled the application to run on that Node
* it configured the cluster to reschedule the instance on a new Node when needed

To verify that the command created a deployment:

`kubectl get deployments`{{execute}}

To see the pods created by the deployment:

`kubectl get pods`{{execute}}

#### The magic of Deployments

If a pod that was created by a Deployment should ever crash, the Deployment will
automatically restart it. To see this in action, kill the pod directly:

`kubectl delete pod $(kubectl get pods --no-headers=true  | awk '{print $1;}')`{{execute}}

The pod should be deleted successfully. Now wait a moment or two and check the pod again:

`kubectl get pods`{{execute}}

Notice the the pod is running again. This is because the Deployment will restart a pod when it fails.

To completely delete the pod and running containers, you must delete the Deployment:

`kubectl delete deployment nginx`{{execute}}
