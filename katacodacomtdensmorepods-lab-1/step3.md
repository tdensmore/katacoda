In Kubernetes, the pods by default can communicate with other pods, regardless
of which host they land on. Every pod gets its own IP address so you do not need
to explicitly create links between pods.

To illustrate this, we will launch a Kubernetes **service** using our NGINX pod.

`kubectl create -f single-container.yaml`{{execute}}

Expose the pod:

`kubectl expose pod nginx --port 80 --name server`{{execute}}

Now create another different pod:

`kubectl create -f alpine-container.yaml`{{execute}}

Now SSH into this container to verify we can `see` the NGINX server:

`kubectl exec -it alpine sh`{{execute}}

then curl the NGINX pod:

`curl -I server `{{execute}}

You should receive a response: `HTTP/1.1 200 OK`
