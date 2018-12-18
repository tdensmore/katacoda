# Lab 1

<!-- `kubeadm init --kubernetes-version $(kubeadm version -o short)`{{execute HOST1}} -->

### Kubernetes

Check the Kubernetes version:

`kubectl version --short`{{execute}}

<!-- `kubectl cluster-info`

`kubectl get nodes` -->

`kubectl get pods`{{execute}}

Notiuce that there are no pods running.

### Run a deployment

Let run an simple NGinx server. To do this in Docker we would do something like this:

`docker run -d --restart=always -e DOMAIN=cluster --name nginx-app -p 80:81 nginx`

but in Kubernetes, we run an nginx Deployment and expose the Deployment like this:

`kubectl run --image=nginx nginx-app --port=81 --env="DOMAIN=cluster"`{{execute}}

`kubectl run --image=sirile/node-image-test --port=8081 --env="DOMAIN=cluster"`{{execute}}

###

`kubectl get rs`

`kubectl get deployments`{{execute}}
