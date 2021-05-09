k get pods 
k get pods --namespace=dev
k create -f x.yaml --namespace=dev
k create -f namespace.yaml
k create namespace dev
k config current-context
k config set-context $(k config current-context) --namespace=dev
k get pods --all-namespaces
