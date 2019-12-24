k create secret generic db-secret --from-file=user_name --from-file=password
k create secret generic rd-secret --from-literal=user_name=admin --from-literal=password=redhat
k get secret
k describe secret rd-secret


NOTE: 
1. Need not to escape special characters in passwords from files (--from-file)
2. Special characters needs to be escaped from command line, simplest way is to surround them via single quotes.
