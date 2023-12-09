# Volumes.

In Kubernetes, a volume is a directory accessible to containers in a pod. It is an abstraction that allows data to persist beyond the lifecycle of the container.

Volumes enable data to be shared between containers in a pod or stored persistently across pod restarts or rescheduling. They play a crucial role in maintaining stateful applications by ensuring that data persists and remains accessible to pods or containers within a Kubernetes cluster.

```
apiVersion: v1
kind: Pod
metadata:
    name: volume-hostpath-test
spec:
    containers:
    - name: busybox
      image: busybox:stable
      volumeMounts:
      - name: host-data
        mountPath: /data
    volumes:
    - name: host-data
      hostPath:
        path: /etc/hostPath
        type: Directory
```

```
apiVersion: v1
kind: Pod
metadata:
    name: empty-dir-test
spec:
    containers:
    - name: busybox
      image: busybox:stable
      volumeMounts:
      - name: empty-dir
        mountPath: /data
    volumes:
    - name: empty-dir
      emptyDir: {}
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

# Persistent Volumes

a Kubernetes persistent volume is like a storage space that stays around even if a Pod using that space is deleted. 
It's a way to store data separately from the containers so that even if the containers restart or are replaced, the data remains accessible.
This persistent volume can be attached or detached from different Pods as needed.

# Persistent Volume Claim

Think of a Kubernetes persistent volume claim as a request for storage by a pod. It's like asking Kubernetes to provide a specific amount and type of storage (like a hard drive) that your application running in the pod needs. Once requested and approved, Kubernetes ensures that the storage is available to the pod as requested, maintaining it even if the pod is removed or rescheduled elsewhere in the cluster.

```
apiVersion: v1
kind: PersistentVolume
metadata:
    name: hostpath-pv
spec:
    capacity:
        storage: 1Gi
    accessMode:
    - ReadWriteOnce
    storageClassName: pv-class
    hostPath:
        path: /etc/hostPath
        type: Directory
```

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: hostpath-pvc
spec:
    accessModes:
    - ReadWriteOnce
    resources:
        requests:
            storage: 200Mi
    storageClassName: slow
```

```
apiVersion: v1
kind: Pod
metadata:
    name: pv-pod-test
spec:
    containers:
    - name: busybox
      image: busybox:stable
      volumeMounts:
      - name: pv-host-data
        mountPath: /data
    volumes:
    - name: pv-host-data
      persistentVolumeClaim:
        claimName: hostpath-pvc
```