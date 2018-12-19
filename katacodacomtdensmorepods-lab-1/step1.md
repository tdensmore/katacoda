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

## Launch Kubernetes Pods

Now we can remove the runnig pod with the command:

`kubectl delete pod nginx-app --now `{{execute}}

We can now verify that the NGiNX pod has been deleted:

To see if any **pods** are currently running:

`kubectl get pods`{{execute}}
