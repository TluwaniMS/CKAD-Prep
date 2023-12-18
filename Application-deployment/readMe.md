# Application Deployment

## Kubernetes deployment:

A Kubernetes Deployment provides instructions to Kubernetes, guiding it on creating or changing instances of pods housing containerized applications.

* ### Getting deployments:

`kubectl get deployments`

* ### Getting a specific deployment by name:

`kubectl get deployment nginx-deployment`

* ### Scaling a deployments replicas:

`kubectl scale deployment/nginx-deployment --replicas=4`

* ### Editing a deployment:

`kubectl edit deployment nginx-deployment`

* ### Show details of a specific deployment:

`kubectl describe deployment nginx-deployment`
