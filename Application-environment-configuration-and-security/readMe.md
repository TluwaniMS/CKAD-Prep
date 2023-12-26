# Application environment, configuration, and security

## Kubernetes API Resources and Custom Resources 

### Kubernetes API Resources

In Kubernetes, an API resource refers to an endpoint within the Kubernetes API that allows you to interact with specific objects or entities in a Kubernetes cluster. These resources represent different aspects of the cluster's desired state, such as pods, services, deployments, configmaps, secrets, and more.

Each API resource has a specific object schema that defines its structure, attributes, and the operations that can be performed on it.

### Kubernetes Custom API Resource:

In Kubernetes, a Custom Resource (CR) or Custom Resource Definition (CRD) allows users to extend the Kubernetes API by defining their custom resources and controllers without modifying the Kubernetes codebase itself.

Custom Resource Definitions (CRDs) enable users to introduce and manage their custom types of resources just like built-in resources (like Pods, Deployments, Services) within a Kubernetes cluster.