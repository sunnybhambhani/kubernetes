# Readiness and Liveness Probes

Luckily Kubernetes have something called Liveness and Readiness probes which makes sure that your application/pod is up and running and is accepting traffic and this helped me to have a proper availability of the application.

### Readiness Probe: 
This means and makes sure that the application is up and running and is ready to accept the traffic. There might be some scenarios in which the application may take some time to get ready meanwhile if in case we don't have a readiness probe in place the service will start routing traffic to the recently initiated pods even though they are not ready to serve the traffic. One of the classic examples is Jenkins in which the web UI is up and running and it takes almost a minute to show the login prompt and accept traffic.

If in the case readiness probe fails it stops sending traffic to the pod until it passes again and if it continues to fail it destroys and recreates the pod once again.

### Liveness Probe: 
This means and makes sure that the application is alive during its lifetime Or does the application require a restart. It works somewhat similar to monit, which automates the restart behavior to make our application up and running. There might be cases in which if the application is up and running since ages it might be that the pod is up and running but the application is not accepting traffic.

### It's kinds (ref. example below for each of the cases, also these three are applicable for both Readiness and Liveness):
* httpGet: It's quite common and can be created custom based on the requirements i.e. Kubernetes will look for the mentioned path(defined in the YAML manifest) and look for its response code. If the response code is in 2XX or 3XX is marked as healthy otherwise it's marked as unhealthy.
* command: This too is quite common it runs a command inside the container and looks for the output. It must always give an exit code 0 to be successful if in-case it's a non-zero value it keeps on restarting the container.
* tcpSocket: It tries to establish a TCP connection on the mentioned port.

### Parameters:
* initialDelaySeconds: Delay in the number of seconds on how much to wait before kicking in the probes (minimum/default is 0).
* periodSeconds: How frequently act (minimum is 1, default is 10).
* timeoutSeconds: Number of seconds after which the probe times out (minimum is 1, default is 1).
* successThreshold: Consecutive successes of the probe to be considered successful after it got failed.
* failureThreshold: How many times Kubernetes will try again before giving up (minimum is 1, default is 3).

### Given below is one simple example stating their usage.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        livenessProbe:
          initialDelaySeconds: 30
          periodSeconds: 10
          tcpSocket:
            port: 80
        readinessProbe:
          initialDelaySeconds: 20
          periodSeconds: 10
          exec:
            command:
            - cat
            - /var/run/nginx.pid
```

Since in the above example we have used nginx, instead of tcpSocket we can use httpGet as well as mentioned below:

```
    livenessProbe:
      httpGet:
        path: /
        port: 80
```

### Important Tip: initialDelaySeconds value should be considered very carefully, since it may lead to a looping restart based on your application behavior. 

Kubernetes have a wonderful document library if interested give it a go.
