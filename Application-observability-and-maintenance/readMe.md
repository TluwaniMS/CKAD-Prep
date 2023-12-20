# Application observability and maintenance

## Kubernetes Probes and Health Checks:

Within Kubernetes, Probes and Health Checks serve as mechanisms to assess the status of containers within a Pod.

There are three main types of probes supported:

1. ### Startup Probe:
This probe is employed to ascertain the initiation of a container application. It assists in delaying the readiness check until specific startup conditions are met.

2. ### Readiness Probe:
Responsible for determining if a container is prepared to handle incoming traffic. Failure in the readiness probe results in the container being unable to receive traffic until it successfully passes the check.

3. ### Liveness Probe:
Used to verify the responsiveness and viability of a container. Should the liveness probe fail, Kubernetes might initiate a container restart.

## Check mechanisms

* ### exec:
Runs a designated command within the container.

* ### grpc:
Conducts a remote procedure call using gRPC.

* ### httpGet:
Initiates an HTTP GET request to the Pod's IP address at a specified port and path.

* ### tcpSocket:
Conducts a TCP check against the Pod's IP address on a specified port.
	


## Configuring Probes:

Example manifests.

```
apiVersion: v1
kind: Pod
metadata:
  name: goproxy
  labels:
    app: goproxy
spec:
  containers:
  - name: goproxy
    image: registry.k8s.io/goproxy:0.1
    ports:
    - containerPort: 8080
    readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 10

```

## Probe fields:

* ### initialDelaySeconds: 

The duration in seconds after the container begins running before the system starts the startup, liveness, or readiness probes.

* ### periodSeconds: 

The frequency, measured in seconds, at which the probe is performed.

* ### timeoutSeconds: 

The maximum duration in seconds allowed for the probe to complete before timing out.

* ### successThreshold: 

The minimum number of consecutive successful probe attempts required for the probe to be marked as successful after a previous failur

* ### failureThreshold: 

When a probe fails a certain number of consecutive times (defined by failureThreshold), Kubernetes deems the overall check as unsuccessful.

* ### terminationGracePeriodSeconds: 

This setting configures a period of time for the kubelet to wait between initiating the shutdown of a failed container and forcibly stopping that container via the container runtime.