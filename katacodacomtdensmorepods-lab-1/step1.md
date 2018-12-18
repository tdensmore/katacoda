# Lab 1

<!-- `kubeadm init --kubernetes-version $(kubeadm version -o short)`{{execute HOST1}} -->

### Kubernetes

Check the Kubernetes version:

`kubectl version --short`{{execute}}

Get information about the cluster:

`kubectl cluster-info`{{execute}}

`kubectl get nodes`{{execute}}

Notice something

`kubectl get pods`{{execute}}

Notice that there are no pods running.

### Run a deployment

Lets run an simple NGinx server. With Kubernetes, we run and expose an nginx **deployment**
using the **run** command.

`kubectl run --image=nginx nginx-app --port=81`{{execute}}

After runing this command, you should see the follwoing:

`deployment.apps/nginx-app created`

### Expose a deployment

`kubectl expose deployment nginx-app --name=nginx-app`{{execute}}

### More stuff

`kubectl get rs`{{execute}}

`kubectl get deployments`{{execute}}

`kubectl get services`{{execute}}

`curl 10.106.239.95`{{execute}}

### Cleanup

`kubectl delete services hello-world`{{execute}}

`kubectl delete deployment hello-world`{{execute}}
