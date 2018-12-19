## Explore the Kubernetes cluster

Check the Kubernetes version:

`kubectl version --short`{{execute}}

Lets see what our cluster looks like.

`kubectl get nodes`{{execute}}

We should have 2 nodes running: **master** and **node1**.

## List Kubernetes Pods

To see if any **pods** are currently running:

`kubectl get pods`{{execute}}

The `No resources found.` message means that there are no pods running.

## Launch Kubernetes Pods

Lets start a pod by deploying a new NGiNX container using the **run** command.

`kubectl run --image=nginx nginx-app --port=81`{{execute}}

After runing this command, you should see the following:

`deployment.apps/nginx-app created`

Now notice that a new pod has been created:

`kubectl get pods`{{execute}}

You should see a running pod similar to this: `nginx-app-55d5c46f74-XXXXX`

NOTE: it may take a few seconds for the pod `STATUS` to change from **ContainerCreating** to **Running**.

## Delete Kubernetes Pods

Now we can remove the running NGiNX pod with the command:

`kubectl delete pod "$(kubectl get pods | awk '{print $1}' | grep nginx)" --now`{{execute}}

NOTE: Since the pod names are dynamic and unique, this command return the current NGiNX pod name:
`$(kubectl get pods | awk '{print $1}' | grep nginx)`

We can now verify that the NGiNX pod has been deleted:

`kubectl get pods`{{execute}}

```
NAME                         READY     STATUS    RESTARTS   AGE
nginx-app-55d5c46f74-XXXXX   1/1       Running   0          13s
```

Wait! Why is the pod is still running?

Answer: When NGiNX was started, Kubernetes created a [deploynment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/). a **deplopyment** helps Kubernetes keep resources available. Kubernetes started a new pod when it realized the NGiNX pod had died.

Verify the running deployment with the command:

`kubectl get deployments`{{execute}}

## Delete Kubernetes Deployments

To really delete the NGiNX pod, we need to delete the **deployment** with the command:

`kubectl delete deployment nginx-app`{{execute}}

You should see this message:

`deployment.extensions "nginx-app" deleted`

Verify this deleted the deployment:

`kubectl get deployments`{{execute}}

And verify this really deleted the pod, too.

`kubectl get pods`{{execute}}
