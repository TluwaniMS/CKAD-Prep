# What is a job?

A finite or batch task that runs to completion.

# What is a cronjob?

A CronJob creates Jobs on a repeating schedule.
CronJob is meant for performing regular scheduled actions 

# Job Manifest Example

```

apiVersion: batch/v1
kind: Job
metadata:
    name: hello-world-job
spec:
    template:
        spec:
            containers:
            - name: hello-world-printer
              image: busybox:stable
              command: ["echo", "Hello world from the Job!"]
            restartPolicy: Never
    backOffLimit: 4
    activeDeadlineSeconds: 10

```

# Cronjob Manifest Example

```

apiVersion: batch/v1
kind: CronJob
metadata:
    name: hello-world-cron-job
spec:
    schedule: "*/1 * * * *"
    jobTemplate:
        spec:
            template:
                containers:
                - name: hello-world-printer
                image: busybox:stable
                command: ["echo", "Hello world from the Cronjob!"]
                restartPolicy: Never
            backOffLimit: 4
            activeDeadlineSeconds: 10

```

# Restart-policy:

The `restartPolicy` is a setting defined within a pod's configuration that specifies the desired behavior for restarting containers within that pod. It determines what action Kubernetes should take if a container within the pod terminates, either due to an error, completion of its task, or any other reason.

This policy allows you to define how resilient your application should be in case of container failures and how Kubernetes manages the lifecycle of your containers within a pod.

There are three main values for `restartPolicy`:

##### 1. Always: 

This setting instructs Kubernetes to always restart the container when it terminates, regardless of the exit code or reason for termination.

##### 2. OnFailure: 

With this setting, Kubernetes restarts the container only if it terminates with an error (non-zero exit code).

##### 3. Never: 

Here, Kubernetes does not restart the container after it terminates, regardless of the exit code or reason for termination. 


# Backofflimit:

In Kubernetes, `backoffLimit` refers to the maximum number of times a controller (like a Job or a CronJob) will retry a failed pod before giving up. When a pod within a controller fails, Kubernetes can automatically retry creating or running that pod based on this `backoffLimit`. If the limit is reached and the pod still fails, the controller stops retrying, marking the overall job as failed. This feature helps manage and control the number of retries for handling pod failures within Kubernetes.

# activeDeadlineSeconds:

The `activeDeadlineSeconds` in Kubernetes is a parameter used within a pod specification to set a maximum amount of time for the pod to execute its primary process. It defines a deadline for the pod's execution duration. If the pod doesn't complete within this timeframe, Kubernetes will mark it as failed and terminate its execution. This parameter is useful for ensuring that pods don't consume resources indefinitely if something goes wrong or if a specific time frame for processing is necessary.

# schedule:

The schedule field in a CronJob determines the frequency and timing of when a particular job or set of tasks should be executed.

The schedule field uses a cron expression to define this timing. In the provided example, "*/1 * * * *" is the cron expression. Each asterisk (*) represents a different time unit:

- The first asterisk represents minutes (runs every minute in this case).
- The second asterisk represents hours.
- The third asterisk represents days of the month.
- The fourth asterisk represents months.
- The fifth asterisk represents days of the week.

So, "*/1 * * * *" means "run this job every minute."

# Practical examples:

#### Active dead line seconds:

Create the active-dead-line-seconds job resource example:

```
kubectl apply -f ./examples/active-dead-line-seconds-example.yaml
```

Retrieve the jobs running in  the cluster:

```
kubectl get jobs
```

Get the job logs of the active-deadline-seconds-example resource:

```
kubectl logs <pod_name>
```


#### backoff limit:

Create the back-off-limit job resource example:

```
kubectl apply -f ./examples/backoff-limit-example.yaml
```

Retrieve the jobs running in  the cluster:

```
kubectl get jobs
```

Get the job logs of the back-off-limit-example resource:

```
kubectl logs <pod_name>
```

#### Cronjob:

Create the Cronjob resource example

```
kubectl apply -f ./examples/cronjob-example.yaml
```

Retrieve the Cronjobs that ran in  the cluster:

```
kubectl get cronjob
```

Get the job logs of the Cronjob resource:

```
kubectl logs <pod_name>
```