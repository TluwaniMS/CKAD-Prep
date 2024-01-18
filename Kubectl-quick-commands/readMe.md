# kubectl commands

The kubectl tool in Kubernetes empowers you to execute commands on Kubernetes clusters and interact with the control plane through the Kubernetes API. With kubectl, you can deploy applications, oversee and manage cluster resources, and examine log information.

`kubectl [command] [TYPE] [NAME] [flags]`

## kubectl create

The kubectl create command generates a resource either from a file or from standard input (stdin).

`kubectl create -f FILENAME`

OR

`kubectl create <resource-type> <resource-name>`

## kubectl run

The kubectl run command creates and rus a particular image in a pod.

```
kubectl run <pod-name> --image=image [--env="key=value"] [--port=port] [--replicas=replicas] [--dry-run=bool] [--overrides=inline-json] [--command] -- [COMMAND] [args...] [—labels="key=value"]
```


