# Multi-container Pods

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
A Pod is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers.

# Multi-container design patterns.

The Kubernetes sidecar design pattern involves attaching additional containers (sidecars) to the main application container within a pod. These sidecars work alongside the primary container, enhancing or extending its functionality without directly altering the main container's code.

This pattern offers several advantages:

### 1. Modularity: 

Sidecars keep different concerns separate. For instance, a logging sidecar can manage log aggregation, leaving the primary container to focus solely on its core function.

### 2. Isolation:

Each container can have its own lifecycle, allowing for independent updates, restarts, or scaling without affecting the primary container.

### 3. Simplicity: Sidecars abstract complex functionalities, making the main application container simpler and more focused on its task.

Examples of sidecar functionalities include logging, monitoring, security, proxying, and networking tasks. For instance, a sidecar container might handle:

- Logging: 

Collecting logs from the main container and forwarding them to a centralized system.

- Monitoring: 

Capturing metrics or health checks for the main application.

- Proxying: 

Acting as an intermediary between the main container and external services.

- Security: 

Handling encryption/decryption, authentication, or authorization tasks.

This pattern promotes flexibility and scalability in Kubernetes deployments, allowing easy addition or removal of sidecar containers to adapt to changing requirements without altering the primary application's codebase.

# Practical examples:

#### Init container example:

Create the init container resource exmaple

```
kubectl apply -f /examples/init-container-example.yaml
```

Check the pod status

```
kubectl get pod init-container-test
```

Check the init container logs

```
kubectl logs init-container-test -c init-container
```

Check the main container logs

```
kubectl logs init-container-test -c displayer-container
```

#### Sidecar container example:

Create the sidecar container resource exmaple

```
kubectl apply -f /examples/side-car-container-example.yaml
```

Check the pod status

```
kubectl get pod sidecar-container-test
```

Check the sidecar container logs

```
kubectl logs sidecar-container-test -c sidecar-container
```

Check the main container logs

```
kubectl logs sidecar-container-test -c main-container
```
