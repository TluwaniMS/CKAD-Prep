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
	