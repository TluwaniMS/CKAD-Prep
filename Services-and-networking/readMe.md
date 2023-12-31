# Services and Networking

## Kuberentes Network Policy:

A Kubernetes network policy outlines the rules for pod communication within a cluster and with external network endpoints. These policies offer precise control over network traffic, facilitating segmentation and security for your applications. They empower you to set specific regulations for both incoming and outgoing traffic of pods.

`NB!`

By default, Pods exist in a state where they can freely communicate with each other. However, when a Kubernetes Network Policy is applied, Pods transition into an isolated state, regulating and restricting their communication within the Kubernetes cluster.

#### Ingress and egress

From a Kubernetes pod's perspective, "ingress" refers to the incoming traffic directed towards the pod, while "egress" is the outgoing traffic originating from the pod.