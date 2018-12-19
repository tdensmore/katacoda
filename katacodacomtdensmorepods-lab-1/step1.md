# Luanching a new Kubernetes Pod

### Explore the Kubernetes cluster

Check the Kubernetes version:

`kubectl version --short`{{execute}}

Lets see what our cluster looks like.

`kubectl get nodes`{{execute}}

We should have 2 nodes running: **master** and **node1**.

### Kubernetes Pods

To see if any **pods** are currently running:

`kubectl get pods`{{execute}}

Notice that there are no pods running ("No resources found.").

Lets start one by deploying a new NGiNX container.

Lets run an simple NGinx server. With Kubernetes, we run and expose an nginx **deployment**
using the **run** command.

`kubectl run --image=nginx nginx-app --port=81`{{execute}}

After runing this command, you should see the following:

`deployment.apps/nginx-app created`

Now notice that a new pod has been created:

`kubectl get pods`{{execute}}

You should see a running pod similar to this: `nginx-app-55d5c46f74-XXXXX`

Now we can remove the runnig pod with the command:

`kubectl delete pod nginx-app --now `{{execute}}
