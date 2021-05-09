NOTE: 
1. Taints are set on nodes and Tolerations are set on pods.
2. Types of effects:
  NoSchedule: Pods will not be scheduled on the node.
  PreferNoSchedule: Try to avoid placing a node.
  NoExecute: New pods will not be placed on the nodes and old pods will be kicked out.

Sample Error: 0/2 nodes are available: 2 node(s) had taints that thepod didn't tolerate

k describe node node01 | grep -i taint
k taint node node01 spray=mortien:NoSchedule
k taint node node01 spray=mortein:NoSchedule --overwrite
k taint node node01 spray-
