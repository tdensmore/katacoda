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

## Launch a Kubernetes Pod

Generally you will want to launch pods into your K8S cluster from a file, since
infrastructure as code promotes transparency and reproducibily.

Examine the `pod.yaml` file in the resource browser.

Launch a new pod in Kubernetes
using the command: Lets start a pod by deploying a new NGiNX container using the **create** command.

`kubectl create -f ./pod.yaml`{{execute}}

Verify that the new NGiNX pod is running:

`kubectl get pods`{{execute}}

It may take a few seconds for the pod `STATUS` to change from **ContainerCreating** to **Running**.

You should see output similar to this:

```
NAME      READY     STATUS    RESTARTS   AGE
nginx     1/1       Running   0          27s
```

## Explore a Kubernetes Pod

`kubectl describe pod nginx`{{execute}}

To get the IP address of the NGiNX pod, use this command:

`kubectl describe pod nginx | grep IP | awk '{print $2}'`{{execute}}

##### Logs

View the internal container logs of the NGiNX pod:

`kubectl logs nginx`{{execute}}

Notice that logfile is empty because NGiNX has served no requests.
To create a log entry, lets request something from NGiNX using the `curl` command:

`curl $(kubectl describe pod nginx | grep IP | awk '{print $2}')`{{execute}}

Now lets look at the logs again:

`kubectl logs nginx`{{execute}}

Notice the new log entry.

##### Log into the pod

`kubectl exec -ti nginx bash`{{execute}}

You are now logged into the running NGiNX container. Poke around and examine the container:

`ls -al`{{execute}}

When you are done, exit the container by typing `exit`.

## Delete a Kubernetes Pod

Now we can remove the running NGiNX pod with the command:

`kubectl delete pod nginx`{{execute}}

We can now verify that the NGiNX pod has been deleted:

`kubectl get pods`{{execute}}

You should see a message confirming that there are no running pods:

```
No resources found.
```
