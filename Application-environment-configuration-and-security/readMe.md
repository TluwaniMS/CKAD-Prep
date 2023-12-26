# Application environment, configuration, and security

## Kubernetes API Resources and Custom Resources 

### Kubernetes API Resources

In Kubernetes, an API resource refers to an endpoint within the Kubernetes API that allows you to interact with specific objects or entities in a Kubernetes cluster. These resources represent different aspects of the cluster's desired state, such as pods, services, deployments, configmaps, secrets, and more.

Each API resource has a specific object schema that defines its structure, attributes, and the operations that can be performed on it.

### Kubernetes Custom API Resource:

In Kubernetes, a Custom Resource (CR) or Custom Resource Definition (CRD) allows users to extend the Kubernetes API by defining their custom resources and controllers without modifying the Kubernetes codebase itself.

Custom Resource Definitions (CRDs) enable users to introduce and manage their custom types of resources just like built-in resources (like Pods, Deployments, Services) within a Kubernetes cluster.

### Custom Resource Definition Example:

```

apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
    name: comeros.super.heroes
spec:
    group: super.heroes
    names:
        plural: comeros
        singular: comero
        kind: ComicHero
        shortNames:
        - cmr
    scope: NameSpaced
    versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
            type: object
            properties:
                hero:
                    type: string
                comic:
                    type: string

```

#### Custom Resource Definition Manifest Walk Through:

```
metadata:
    name: comic.super.heroes
```

The name should follow the following format: `<plural>.<group>`

```
group: super.heroes
```

The group name to use for REST API: `/apis/<group>/<version>`

```
names:
```

Defines the names used to identify this custom resource.

```
plural: comeros
```

The plural name to be used in the URL: `/apis/<group>/<version>/<plural>`

```
singular: comero
```

The singular name to be used as an alias on the CLI and for display

```
kind: ComicHero
```

This refers to the kind of resource used in Kubernetes manifests, to identify your resource.

```
shortNames:
- cmr
```

shortNames allow shorter string to match your resource on the CLI

```
scope: NameSpaced
```

Defines the scope of the resource.

```
versions:
```

Describes the versions of this custom resource.

```
served: true
```

Served indicates whether this version should be served via the API server or not.

```
storage: true
```

Storage indicates whether resources of this version should be persisted or not. 

```
schema
```

Defines the OpenAPI v3 schema for validation of this version of the custom resource.

