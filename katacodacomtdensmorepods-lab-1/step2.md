## Multi-Container Pods


#### Communication between Pods

#### Shared Volumes

In Kubernetes, you can use a shared Kubernetes Volume as a simple and efficient
way to share data between containers in a Pod. For most cases, it is sufficient
to use a directory on the host that is shared with all containers within a Pod.

Kubernetes Volumes enables data to survive container restarts, but these volumes have the same lifetime as the Pod. That means that the volume (and the data it holds) exists exactly as long as that Pod exists. If that Pod is deleted for any reason, even if an identical replacement is created, the shared Volume is also destroyed and created anew.

A standard use case for a multi-container Pod with a shared Volume is when one container writes logs or other files to the shared directory, and the other container reads from the shared directory. For example, we can create a Pod like so:

To this point we have been working with a pod hosting a single container.
However, some container patterns (sidecar, adapter and ambassador) require more than one running container.

#### IPC

The **sidecar** container pattern is a very common practice for logging utilities, sync services, watchers, and monitoring agents.

## Launch a Multi-Container Pod

Generally you will want to launch pods into your K8S cluster from a file, since
infrastructure as code promotes transparency and reproducibily.

Examine the `multi-container.yaml` file in the resource browser.

This file specifies a simple Alpine Linux container and an NGiNX container.

Launch a new pod in Kubernetes
using the command: Lets start a pod by deploying a new NGiNX container using the **create** command.

`kubectl create -f ./multi-container.yaml`{{execute}}

NOTE: If you receive this message: `error: the path "./multi-container.yaml" does not exist`, click
on the file `multi-container.yaml` in the explorer window above the terminal window
to the right of this pane, then retry the command.

Verify that the new `pod-with-sidecar` pod is running:

`kubectl get pods`{{execute}}

It may take a few seconds for the pod `STATUS` to change from **ContainerCreating** to **Running**.

You should see output similar to this:

```
NAME               READY     STATUS    RESTARTS   AGE
pod-with-sidecar   2/2       Running   0          15s
```

## Explore a Kubernetes Pod

`kubectl describe pod nginx`{{execute}}

To get the IP address of the NGiNX pod, use this command:

`kubectl describe pod nginx | grep IP | awk '{print $2}'`{{execute}}

##### View pod logs

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

Now we can remove the running NGiNX pod using the YAML file:

`kubectl delete -f ./single-pod.yaml`{{execute}}

or directly with this command:

`kubectl delete pod nginx`{{execute}}

We can now verify that the NGiNX pod has been deleted:

`kubectl get pods`{{execute}}

You should see a message confirming that there are no running pods:

```
No resources found.
```

## Summary

We learned about Kubernetes Pods: how to create them, interact with them, and remove them.

In the next lesson, we will explore multi-container pods.
