# Services and Networking

## Kuberentes Network Policy:

A Kubernetes network policy outlines the rules for pod communication within a cluster and with external network endpoints. These policies offer precise control over network traffic, facilitating segmentation and security for your applications. They empower you to set specific regulations for both incoming and outgoing traffic of pods.

`NB!`

By default, Pods exist in a state where they can freely communicate with each other. However, when a Kubernetes Network Policy is applied, Pods transition into an isolated state, regulating and restricting their communication within the Kubernetes cluster.

#### Ingress and egress

From a Kubernetes pod's perspective, "ingress" refers to the incoming traffic directed towards the pod, while "egress" is the outgoing traffic originating from the pod.

##### Example Manifest for Network Policy that denies all ingress traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

##### Example Manifest for Network Policy that permits all ingress traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-ingress
spec:
  podSelector: {}
  ingress:
  - {}
  policyTypes:
  - Ingress
```

##### Example Manifest for Network Policy that denies all egress traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress
spec:
  podSelector: {}
  policyTypes:
  - Egress
```

##### Example Manifest for Network Policy that permits all egress traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-egress
spec:
  podSelector: {}
  egress:
  - {}
  policyTypes:
  - Egress
```

##### Example Manifest for Network Policy that denies all ingress and egress traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

##### Example Network Policy manifest with specific selectors for inbound traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np-test-client-allow
  namespace: np-test-a
spec: 
  podSelector:
    matchLabels:
      app: np-test-server
  policyTypes:
  - Ingress
  Ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          team: bteam
      podSelector:
        matchLabels:
          app: np-test-client
    ports:
    - protocol: TCP
      port: 80
```

##### Example Network Policy manifest with specific selectors for inbound traffic walk through:

* Specifies the pods to which this policy applies. Matches pods labeled with `app: np-test-server`.

```
spec: 
  podSelector:
    matchLabels:
      app: np-test-server
```

* Specifies the types of policies this NetworkPolicy applies to.

```
policyTypes:
```

* Rules for incoming traffic. Allows traffic from specific sources based on selectors and ports.

```
Ingress:
```

* Specifies the sources allowed to send traffic.

```
- from:
```

* Allows traffic from pods within namespaces that have a label team: bteam. 

```
- namespaceSelector:
    matchLabels:
    team: bteam
```

* Specifically allows traffic from pods labeled with app: np-test-client.

```
podSelector:
  matchLabels:
    app: np-test-cli
```

* Defines the ports and protocols allowed for incoming traffic.Allows TCP traffic on port 80.

```
ports:
- protocol: TCP
  port: 80
```


`Summary of manifest:`

This NetworkPolicy is designed to allow inbound traffic from pods labeled app: np-test-client within namespaces that have the label team: bteam, specifically on TCP port 80, and it applies to pods labeled as app: np-test-server within the "np-test-a" namespace.

##### Example Network Policy manifest with specific selectors for outbound traffic:

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: np-test-client-allow-egress
  namespace: np-test-b
spec:
  podSelector:
    matchLabels:
      app: np-test-client
  policyTypes:
  - Egress
  Egress:
  - to:
    - namespaceSelector:
        matchLabels:
          team: ateam
    ports:
    - protocol: TCP
      port: 80
```

* Specifies the destination for the allowed traffic. Allows traffic to pods within namespaces labeled with `team: ateam`.

```
Egress:
- to:
  - namespaceSelector:
      matchLabels:
          team: ateam
```

`Summary of manifest:`

This NetworkPolicy specifically targets pods labeled as app: np-test-client within the "np-test-b" namespace. It restricts these pods' outgoing traffic (Egress) by allowing communication limited to namespaces labeled as team: ateam on TCP port 80. Essentially, it permits pods with the specified label to send TCP traffic to pods in namespaces labeled as team: ateam on port 80.

## Services

In Kubernetes, a Service functions as a means to make a network application, which operates within your cluster as one or more Pods, accessible to the network.

### Types of Services:

* ClusterIP (default):

This type of Service exposes it on an internal IP within the cluster, restricting accessibility solely to within the cluster.

* NodePort:

By employing NAT, this Service type exposes the Service on the identical port of each chosen Node in the cluster. Consequently, it enables the Service to be reached from outside the cluster through <NodeIP>:<NodePort>, encompassing the functionalities of ClusterIP.


