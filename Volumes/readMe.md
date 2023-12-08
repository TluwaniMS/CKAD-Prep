# Volumes.

In Kubernetes, a volume is a directory accessible to containers in a pod. It is an abstraction that allows data to persist beyond the lifecycle of the container.

Volumes enable data to be shared between containers in a pod or stored persistently across pod restarts or rescheduling. They play a crucial role in maintaining stateful applications by ensuring that data persists and remains accessible to pods or containers within a Kubernetes cluster.

```
Manifest example 1
```

```
Manifest example 2
```

* The Pod spec's `volumes` field specifies information regarding the volumes employed within the Pod.
* Within the container spec, the `volumeMounts` field links a volume to a designated container at a particular path.
* `hostPath` volumes facilitate the mounting of data from a defined location on the host (k8s node).

##### hostPath volume types:

* `Directory` – Attaches an already existing directory from the host.
* `DirectoryOrCreate` – Attaches a directory from the host and generates it if it's not present.
* `File` – Attaches an already existing individual file from the host.
* `FileOrCreate` – Attaches a file from the host and generates it if it's not present.

* `emptyDir` volumes offer temporary storage utilizing the host file system, and they are deleted when the Pod is removed.