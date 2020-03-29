# Configmaps

### Commands
```
k create configmap env-nginx --from-literal=NGINX_HOME=/opt/nginx/current --from-literal=NGINX_BIN=/opt/nginx/current/bin
k get configmaps
k describe configmap env-nginx
k create configmap env-nginx --from-file nginx_conf
```

### Note:
* valueFrom will inject the value of a key from the referenced configMap.
* envFrom will inject all configMap keys as environment variables.

