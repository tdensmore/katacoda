In ours earlier Pods lab, we explored running pods. Pods are very useful for
providing structure to containers, but if a pod fails it will not be restarted.

To remedy this, Kubernetes created an object called a `Deployment`. Deployments
are specifications for running one or more pods, and methods to **keep them running**.

To run a Kubernetes deployment from the command-line:

`kubectl run nginx --image=nginx --port 80`{{execute}}

Congrats, you have just created your first pod deployment. The `run` command
automatically performed a few things for you:

* it searched for a suitable node to run the pod
* it scheduled the pod to run on that Node
* it configured the cluster to restart / reschedule the pod when needed

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
