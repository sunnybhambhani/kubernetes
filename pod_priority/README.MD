# Pod Priority

```
k apply -f pods_before.yaml
```

```
master $ k get pods --show-labels
NAME            READY   STATUS    RESTARTS   AGE   LABELS
dev-nginx       1/1     Running   0          55s   env=dev
preprod-nginx   1/1     Running   0          55s   env=preprod
prod-nginx      0/1     Pending   0          55s   env=prod
```

### Since we don't have enough resources one of our PROD pod went into PENDING state.

```
master $ k get events
...
...
5s          Warning   FailedScheduling   pod/prod-nginx      0/2 nodes are available: 1 Insufficient cpu, 2 Insufficient memory.
```

### If you will see the events it will clrealy state about the resource contraints.

```
master $ ls -lrt
-rw-r--r-- 1 root root  195 Mar 29 12:42 dev-pc.yaml
-rw-r--r-- 1 root root  202 Mar 29 12:42 preprod-pc.yaml
-rw-r--r-- 1 root root  232 Mar 29 12:40 prod-pc-p1.yaml
-rw-r--r-- 1 root root  245 Mar 29 12:40 prod-pc-p2.yaml
```
### Here comes the PriorityClass in picture, to try its best and make sure that our top priority pods are up and running.

```
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: prod-pc-p2
value: 3000000
globalDefault: false
description: "This priority class should only be used for all rest of the production pods like backend, frontend servers etc."
```

```
master $ k apply -f .
priorityclass.scheduling.k8s.io/dev-pc created
priorityclass.scheduling.k8s.io/preprod-pc created
priorityclass.scheduling.k8s.io/prod-pc-p1 created
priorityclass.scheduling.k8s.io/prod-pc-p2 created

master $ k get pc
NAME                      VALUE        GLOBAL-DEFAULT   AGE
dev-pc                    1000000      false            3m13s
preprod-pc                2000000      false            2m3s
prod-pc-p1                4000000      false            7s
prod-pc-p2                3000000      false            7s
system-cluster-critical   2000000000   false            136m
system-node-critical      2000001000   false            136m
```

### system-cluster-critial and system-node-critical are the default ones and are basically used by the pods in kube-system namespace.
### Updated the pod to have a priority class assigned to PROD.
### Deleted the PROD pod and recreated again using the priority class.
### priorityClassName helps in pod manifect to use the priority class.

```
master $ k get pods --show-labels
NAME            READY   STATUS    	RESTARTS   AGE   LABELS
dev-nginx       1/1     Terminating	0          55s   env=dev
preprod-nginx   1/1     Running   	0          55s   env=preprod
prod-nginx      0/1     Pending   	0          55s   env=prod
```

### One of the pod is now in terminating state, once that is terminated our PROD pod will become available to us. 

```
Warning   FailedScheduling   pod/prod-nginx    0/2 nodes are available: 1 Insufficient cpu, 2 Insufficient memory.
Normal    Preempted          pod/dev-nginx     by default/prod-nginx on node node01
Normal    Killing            pod/dev-nginx     Stopping container dev-nginx
Normal    Scheduled          pod/prod-nginx    Successfully assigned default/prod-nginx to node01
Normal    Pulling            pod/prod-nginx    Pulling image "nginx"
Normal    Pulled             pod/prod-nginx    Successfully pulled image "nginx"
Normal    Created            pod/prod-nginx    Created container prod-nginx
Normal    Started            pod/prod-nginx    Started container prod-nginx
```

### As the events states it evicted the dev-nginx pod and instead of that it scheduled the prod-nginx pod over node01.
