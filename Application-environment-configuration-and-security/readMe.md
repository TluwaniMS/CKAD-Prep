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
subjects:
```

* Specifies the kind of the subject being bound.

```
- kind: ServiceAccount
```

* Specifies the name of the ServiceAccount.

```
name: service-account-name
```

* Specifies the namespace in which the ServiceAccount resides.

```
namespace: default
```

* Specifies the Role being referenced and bound to the subjects.

```
roleRef:
```

* Indicates the kind of resource to which the RoleBinding refers. 

```
Kind: Role
```

* Specifies the name of the Role to be bound.

```
name: list-pods-role
```

* Indicates the API group to which the referenced Role belongs.

```
apiGroup: rbac.authorization.k8s.io
```

## Kubernetes Admission Controllers

An admission controller refers to a code segment that intercepts requests made to the Kubernetes API server before the object is stored. This interception occurs subsequent to authentication and authorization of the request.

These controllers function akin to gatekeepers, possessing the capability to modify the request object or decline the request entirely.

## Managing Compute Resource Usage

### Resource Quota:

A ResourceQuota object establishes limitations on overall resource usage within a namespace. It regulates the number of objects allowed per type and controls the total compute resources utilized by the resources within that specific project.

`NB!`

In numerous Kubernetes distributions, Resource Quota support is automatically activated. This happens when the API server includes the 'ResourceQuota' as one of its arguments in the '--enable-admission-plugins=' flag.

#### Example Pod manifest with resource requests and limits:

```
apiVersion: v1
kind: Pod
metadata:
    name: pod-resource-management
spec:
    containers:
    - name: busybox
      image: busybox:stable
      resources:
        requests:
            memory: 64Mi
            cpu: 250m
        limits:
            memory: 128Mi
            cpu: 500m
```

#### Example Pod manifest walk through:

* Limits:

define the maximum amount of resources a container can use.

* Requests:

define the minimum amount of resources that should be reserved for a container.

#### Example Resource Quota manifest:

```
apiVersion: v1
kind: ResourceQuota
metadata:
    name: test-resource-quota
    namespace: test-namespace
spec:
    hard:
        requests.memory: 128mi
        requests.cpu: 500m
        limits.memory: 256mi
        limits.cpu: '1'
```

#### Example ResourceQuota manifest walk through:

* Limits:

define the maximum amount of resources for the namespace.

* Requests:

define the minimum amount of resources that should be reserved for the namespace.


# Configuring Applications with ConfigMaps and Secrets

## Configmap

A ConfigMap is an API element designed for storing non-sensitive information in the form of key-value pairs. Pods have the capability to utilize ConfigMaps either as environment variables, command-line arguments, or configuration files within a volume.

#### Configmap Manifest Example:

```

apiVersion: v1
kind: ConfigMap
metadata:
    name: test-configmap
data:
    instance: dev
    app.cfg: |
        key1=value
        key2=value

```

#### Configmap Manifest walkthrough:

* This section contains the actual key-value pairs that constitute the data stored within the ConfigMap.

```
data:
```

* property-like keys; each key maps to a simple value

```
instance: dev
```

* file-like keys

```
app.cfg: |
```

#### Example of Pod manifest reading from configmap:

```

apiVersion: v1
kind: Pod
metadata:
    name: config-pod
spec:
    containers:
    - name: busybox
      image: busybox:stable
      env:
      - name: MESSAGE
      valueFrom:
        configMapKeyRef:
            name: <configmap-name>
            key: <key-in-configmap>
      volumeMounts:
      - name: <volume-name>
        mountPath: /config
        readOnly: true
    volumes:
    - name: <volume-name>
      configMap:
        name: <configmap-name>
        items:
        - key: <file/key-name>
          path: <path-to-access-key>

```

#### Pod manifest walkthrough:

* Specifies environment variables for the container.

```
env:
```

* Defines an environment variable named "MESSAGE" 

```
- name: MESSAGE
```

* This specifies where the environment variable is sourced from.

```
valueFrom:
```

* Specifies that the value of the environment variable comes from a specific key within a ConfigMap.

```
configMapKeyRef:
```

* The name of the ConfigMap containing the desired key-value pair.

```
name: <configmap-name>
```

* The specific key in the ConfigMap whose value will be used for the "MESSAGE" environment variable.

```
key: <key-in-configmap>
```

* Specifies that the volume source is a ConfigMap.

```
configMap:
```

* The name of the ConfigMap to be used as the volume.

```
name: <configmap-name>
```

*  Specifies particular items from the ConfigMap to be used within the volume.

```
items:
```

* The specific key in the ConfigMap that will be used within the volume.

```
key: <file/key-name>
```

* Specifies the path within the volume where the ConfigMap key will be accessible.

```
path: <file/key-name>
```

## Secrets

A Secret is an element designed to store small, sensitive information like passwords, tokens, or keys. It's akin to ConfigMaps but is specifically designed to securely manage confidential data.

#### Secret Manifest Example:

`NB!`

Secret(s) data is base64-encoded.

```
apiVersion: v1
kind: Secret
metadata:
    name: secret-test
type: Opaque
data:
    sensitive.data: U2VjcmV0IFN0dWZmIQo=
    password.txt: U2VjcmV0IHN0dWZmIGluIGEgZmlsZSEK
```

#### Example of Pod manifest reading from secret:

```

apiVersion: v1
kind: Pod
metadata:
    name: secret-pod
spec:
    containers:
    - name: busybox
      image: busybox:stable
      env:
      - name: MESSAGE
      valueFrom:
          secretKeyRef:
            name: <secret-name>
            key: <key-in-secret>
      volumeMounts:
      - name: <volume-name>
        mountPath: /secret
        readOnly: true
    volumes:
    - name: <volume-name>
      secret:
        secretName: <secret-name>
        items:
        - key: <file/key-name>
          path: <path-to-access-key>

```

#### Pod manifest walkthrough:

* Specifies that the value of the environment variable comes from a specific key within a Secret.

```
secretKeyRef:
```

* Specifies that the volume source is a Secret.

```
secret:
```

* The name of the Secret to be used as the volume.

```
secretName: <secret-name>
```

## Kubernetes Security Context

A Kubernetes security context involves a collection of security configurations implemented either at the pod or container level, allowing the specification of privileges and access controls for these pods or containers.





