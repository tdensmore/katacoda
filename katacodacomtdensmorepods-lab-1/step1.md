# Lab 1

<!-- `kubeadm init --kubernetes-version $(kubeadm version -o short)`{{execute HOST1}} -->

### Kubernetes

Check the Kubernetes version:

`kubectl version --short`{{execute}}

Get information about the cluster:

`kubectl cluster-info`{{execute}}

`kubectl get nodes`{{execute}}

`kubectl get pods`{{execute}}

Notiuce that there are no pods running.

### Run a deployment

Let run an simple NGinx server. To do this in Docker we would do something like this:

`docker run -d --restart=always -e DOMAIN=cluster --name nginx-app -p 80:81 nginx`

but in Kubernetes, we run an nginx Deployment and expose the Deployment like this:

`kubectl run --image=nginx nginx-app --port=81 --env="DOMAIN=cluster"`{{execute}}

or this

`kubectl run --image=sirile/node-image-test image-test --port=8080 --env="DOMAIN=cluster"`{{execute}}

`kubectl run hello-world --image=gcr.io/google-samples/node-hello:1.0 --port=8080`{{execute}}

`kubectl expose deployment image-test --port=80 --name=image-test`{{execute}}

`kubectl expose deployment image-test --port 80 --name=image-test --target-port 8080`{{execute}}

### More stuff

`kubectl get rs`{{execute}}

`kubectl get deployments`{{execute}}

`kubectl get services`{{execute}}

`curl 10.106.239.95`{{execute}}
