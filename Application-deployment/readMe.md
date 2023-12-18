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

## Managing an update rollout:

* ### Check the deployments rollout history:
  
`kubectl rollout history deployment/<deployment-name>`

* ### Undo a deployment rollout:
  
`kubectl rollout undo deployment/<deployment-name>`

* ### Check the status of a deployment rollout:
  
`kubectl rollout status deployment/<deployment-name>`

## Deployment strategy(s)

* ### Blue/Green Deployment strategy:
  
The system operates with two production environments, blue and green, for software updates. Blue represents the current software version, while green hosts the new one. At any given moment, only one environment is active, handling all production traffic. Once the new version successfully passes tests, traffic is directed to the new environment. In case of any issues, traffic reverts to the previous version.

* ### Canary deployment strategy:

A canary deployment involves gradually introducing a new version of an application by directing a portion of the traffic to this new version while still maintaining the existing version for the majority of users, enabling a phased rollout before complete implementation.
