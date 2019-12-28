Note:
1. New deployment creates a rollout.
2. New rollout creates a new revision (ex: 1)
3. When applition is upgraded a new rollout is triggered and a new revision is created.
4. This helps to move back and forth with out deployments.
5. Strategies: ReCreate(with downtime) and RollingUpdate(without downtime and is the default one).


k rollout status deployment nginx
k rollout history deployment nginx 
k rollout undo deployment nginx #Undo a change to the last revision
k rollout history deployment/nginx --revision 2 #See more information about the revision.
k rollout undo deployment nginx --to-revision 2 #To move back the deployment to revision 2.

strategy:
  rollingUpdate:
  maxSurge: 25%
  maxUnavailable: 25%
type: RollingUpdate
