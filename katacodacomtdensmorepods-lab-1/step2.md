

To this point we have been working with a pod hosting a single container.
However, some container patterns (sidecar, adapter and ambassador) require more
than one running container.

NOTE: the **sidecar** container pattern is a very common practice for logging
utilities, sync services, watchers, and monitoring agents.

## Launch a Multi-Container Pod

Generally you will want to launch pods into your K8S cluster from a file, since
infrastructure as code promotes transparency and reproducibily.

Examine the `multi-container.yaml` file in the resource browser.

This file specifies a simple Alpine Linux container and an NGiNX container.
Start a pod by deploying a new NGiNX container using the **create** command.

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

## Label the Pod

One or more labels can be applied to a pod. Apply a label to the sidecar pod:

`kubectl label pods pod-with-sidecar containers=2`{{execute}}

To show labels on a pod, use the following command:

`kubectl get pods --show-labels`{{execute}}

## List all containers running in a pod

To show the container names associated with the pod labelled "containers=2":

`kubectl get pods -l containers=2 -o jsonpath={.items[*].spec.containers[*].name}`{{execute}}

You should see the following container names:

* app-container
* sidecar-container

## Communication between Containers in a Pod

#### Shared Volumes

In Kubernetes, you can use a shared Kubernetes Volume as a simple and efficient
way to share data between containers in a Pod. For most cases, it is sufficient
to use a directory on the host that is shared with all containers within a Pod.

Kubernetes Volumes enables data to survive container restarts, but these volumes have the same lifetime as the Pod. That means that the volume (and the data it holds) exists exactly as long as that Pod exists. If that Pod is deleted for any reason, even if an identical replacement is created, the shared Volume is also destroyed and created anew.

A standard use case for a multi-container Pod with a shared Volume is when one container writes logs or other files to the shared directory, and the other container reads from the shared directory.

Examine the file "pod-volume.yaml" in the file explorer.

The 1st container runs nginx server and has the shared volume mounted to the
directory ``/usr/share/nginx/html`.

The 2nd container uses a Debian image and mountes the shared volume to the
container directory `/html`.

Every second, the 2nd container adds the current date and time into the
`index.html` file, which is located in the shared volume. When the user makes
an HTTP request to the Pod, the Nginx server reads this file and transfers it
back to the user in response to the request.

`kubectl create -f ./pod-volume.yaml`{{execute}}

#### IPC

Containers in a Pod share the same IPC namespace, which means they can also
communicate with each other using standard inter-process communications such
as SystemV semaphores or POSIX shared memory.

Examine the file "pod-volume.yaml" in the file explorer.

The first container, producer, creates a standard Linux message queue, writes a
number of random messages, and then writes a special exit message. The second
container,  consumer, opens that same message queue for reading and reads
messages until it receives the exit message.

Run the example:

`kubectl create -f ./pod-ipc.yaml`{{execute}}

When the pod is running check the logs of each container:

`kubectl logs pod-ipc -c producer`{{execute}}

`kubectl logs pod-ipc -c consumer`{{execute}}

Delete the pod when you are done:

`kubectl delete -f ./pod-ipc.yaml`{{execute}}

## Summary

We explored multi-container Kubernetes Pods, and explored container
communication within a pod.

In the next lesson, we will explore multi-pod communication.
