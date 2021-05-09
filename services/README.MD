k get services
k describe service webapp-service 
k expose deployment/simple-webapp-deployment --type=NodePort --port=8080 --target-port=8080 --name=webapp-service
k edit service webapp-service


Note: 
1. We cannot have node port in the imperitive command instead need to edit the service or we can use the declarative way.
2. There are 3 types of services, NodePort, ClusterIP, LoadBalancer
