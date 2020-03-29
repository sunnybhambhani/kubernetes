# Pods

Pods are the smallest unit in Kubernetes.

### Commands
```
k get pods
k run nginx --image=nginx:latest
k run --generator=run-pod/v1 nginx --image=nginx:latest
k delete pods nginx
k describe pods newpods | grep -i image:
k describe pods newpods | grep -i node:
k describe pod webapp | grep 'Container ID'
k describe pod webapp | grep -i image:
k run redis --image=redis --generator=run-pod/v1
k edit pods redis
```

* I have an alias for kubectl as k, therefore all the commands states k.

### [Sample Single Pod File] https://github.com/sunnybhambhani/kubernetes/blob/master/pods/sample_pod.yaml
### [Sample Multi Pod File] https://github.com/sunnybhambhani/kubernetes/blob/master/pods/sample_multi_pod.yaml
