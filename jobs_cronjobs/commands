Note:
1. Workloads, life span is based on the task which is performed.
2. restartPolicy: Never (by defauly it is set to Always).
3. apiVersion: batch/v1 
4. king: Job
5. completions: 3 (create 3 pods one after another).
6. parallelism: 3 (created 3 pods in parallel).

k create -f sample_2.yaml
k get jobs
k describe job math-job
k logs math-expr
k delete job math-job


7. Similar to cron in linux
8. schedule: <CRON components>
9. jobTemplate: <Reference from Job>
10. Be careful since now there are three spec section, one for cronjob, one for job, one for pod.

k create -f sample_3.yaml
k get cronjobs
k describe cronjob math-cronjob
k delete cronjob math-cronjob
