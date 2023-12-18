# Application Deployment

## Kubernetes deployment:

A Kubernetes Deployment provides instructions to Kubernetes, guiding it on creating or changing instances of pods housing containerized applications.

* ### Getting deployments:

`kubectl get deployments`

* ### Getting a specific deployment by name:

`kubectl get deployment <deployment-name>`

* ### Scaling a deployments replicas:

`kubectl scale deployment/<deployment-name> --replicas=4`

* ### Editing a deployment:

`kubectl edit deployment <deployment-name>`

* ### Show details of a specific deployment:

`kubectl describe deployment <deployment-name>`

## Rolling updates:

Rolling updates enable Deployments to update without causing any downtime by gradually replacing existing Pod instances with new ones.

* ### Update the deployments image:
`kubectl set image deployment/<deployment-name> <container>=<image>:<tag>`

OR

`kubectl set image deployment <deployment-name> <container>=<image>:<tag>`
