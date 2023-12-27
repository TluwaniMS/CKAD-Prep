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

* The name should follow the following format: `<plural>.<group>`

```
metadata:
    name: comic.super.heroes
```

* The group name to use for REST API: `/apis/<group>/<version>`

```
group: super.heroes
```

* Defines the names used to identify this custom resource.

```
names:
```

* The plural name to be used in the URL: `/apis/<group>/<version>/<plural>`

```
plural: comeros
```

* The singular name to be used as an alias on the CLI and for display

```
singular: comero
```

* This refers to the kind of resource used in Kubernetes manifests, to identify your resource.

```
kind: ComicHero
```

* shortNames allow shorter string to match your resource on the CLI

```
shortNames:
- cmr
```

* Defines the scope of the resource.

```
scope: NameSpaced
```

* Describes the versions of this custom resource.

```
versions:
```

* Served indicates whether this version should be served via the API server or not.

```
served: true
```

* Storage indicates whether resources of this version should be persisted or not. 

```
storage: true
```

* Defines the OpenAPI v3 schema for validation of this version of the custom resource.

```
schema
```

#### Custom Resource Example:

```

apiVersion: super.heroes/v1
kind: ComicHero
metadata:
    name: dombo
spec:
    hero: Changamire
    comic: Barodzwi

```

## Service Accounts

A ServiceAccount establishes an identity for the operations carried out by processes within a Pod.

Processes within a Pod have the ability to employ the identity linked to their corresponding service account for authentication towards the cluster's API server.

#### Service Account Example:

```

apiVersion: v1
kind: ServiceAccount
metadata:
    name: service-account-name
automountServiceAccountToken: true

```

The `automountServiceAccountToken` field in a Kubernetes ServiceAccount manifest specifies whether to automatically mount the associated service account's credentials (token) into the running Pod's filesystem.

When set to true, it enables the automatic mounting of the service account token into the default location within the Pod's filesystem. This token can then be used by processes running within the Pod to authenticate themselves to the Kubernetes API server.

#### Adding service account to Pod:

```
apiVersion: v1
kind: Pod
metadata:
    name: service-account-pod
spec:
    serviceAccountName: service-account-name
    conatiners:
    - name: <container-name>
    .....

```


## Kubernetes RBAC

Within Kubernetes, ClusterRoles and Roles establish the permissions granted to users within a cluster or a namespace, respectively. These permissions can be assigned to various Kubernetes subjects such as users, groups, or service accounts using role bindings and cluster role bindings.

#### Kubernetes Role example:

```

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    name: list-pods-role
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list"]

```

#### kubernetes Role Manifest walk through:

* The rules describe the permissions or rules defined by this Role.

```
rules:
```

* Indicates the API groups to which the permissions apply. Here, [""] (an empty string) signifies the core Kubernetes API group.

```
apiGroups: [""]
```

* This specifies the Kubernetes resources the Role has permissions for.

```
resources: ["pods"]
```

* This defines the actions or operations permitted on the specified resources. 

```
verbs: ["list"]
```

#### Kubernetes Role binding example:

```

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: list-pods-rb
subjects:
- kind: ServiceAccount
  name: service-account-name
  namespace: default
roleRef:
  Kind: Role
  name: list-pods-role
  apiGroup: rbac.authorization.k8s.io

```

#### kubernetes RoleBinding manifest walk through:

*  Defines the subjects to which the Role is bound.

```
```

* Specifies the kind of the subject being bound.

```
```

* Specifies the name of the ServiceAccount.

```
```

* Specifies the namespace in which the ServiceAccount resides.

```
```

* Specifies the Role being referenced and bound to the subjects.

```
```

* Indicates the kind of resource to which the RoleBinding refers. 

```
```

* Specifies the name of the Role to be bound.

```
```

* Indicates the API group to which the referenced Role belongs.

```
```