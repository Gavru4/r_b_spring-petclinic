### Creation of StatefulSet for Redis cluster

1. Created a PVC for saving data (`redis-pvc.yaml`)
2. Headless Service created (`redis-service.yaml`)
3. Created StatefulSet with 2 replicas (`redis-statefulset.yaml`)
4. Checking:

```bash command that i used
kubectl apply -f redis-pvc.yaml   result =>=> "persistentvolumeclaim/redis-data created"
kubectl apply -f redis-service.yaml  result =>=> "service/redis created"
kubectl apply -f redis-statefulset.yaml   result =>=> "statefulset.apps/redis created"
kubectl get pods  result =>=>
   # NAME      READY   STATUS    RESTARTS   AGE
   # redis-0   1/1     Running   0          14s
   # redis-1   1/1     Running   0          5s
kubectl exec -it redis-0 -- redis-cli
  ## inside the pod :
  set testkey "hello-from-redis-0"
  get testkey  result =>=>=> "hello-from-redis-0"

  ##  then
  kubectl delete pod redis-0
  kubectl exec -it redis-0 -- redis-cli
  get testkey
     result =>=>=> "hello-from-redis-0"

##  Summary: restarting redis-0 does not result in data loss.
```

## Setup Falco in Kubernetes using DaemonSet

1. Created falco-daemonset.yaml with all necessary mounts
2. Limited resources: 100m CPU, 256Mi memory
3. Checking:

```bash command that i used
kubectl apply -f falco-daemonset.yaml   result =>=> "daemonset.apps/falco created"
kubectl get pods -n kube-system -l app=falco =>=>
   # NAME      READY   STATUS    RESTARTS   AGE
   # falco-x66k5   1/1     Running   0          68s

kubectl logs -n kube-system -l app=falco
#  result =>=>=>
# 2025-05-25T17:31:34+0000: System info: Linux version 6.10.14-linuxkit (root@buildkitsandbox) (gcc (Alpine 13.2.    1_git20240309) 13.2.1 20240309, GNU ld (GNU Binutils) 2.42) #1 SMP Mon Feb 24 16:35:16 UTC 2025
# 2025-05-25T17:31:34+0000: Loading rules from:
# 2025-05-25T17:31:36+0000:    /etc/falco/falco_rules.yaml | schema validation: ok
# 2025-05-25T17:31:36+0000:    /etc/falco/falco_rules.local.yaml | schema validation: none
# 2025-05-25T17:31:36+0000: The chosen syscall buffer dimension is: 8388608 bytes (8 MBs)
# 2025-05-25T17:31:36+0000: Starting health webserver with threadiness 8, listening on 0.0.0.0:8765
# 2025-05-25T17:31:36+0000: Loaded event sources: syscall
# 2025-05-25T17:31:36+0000: Enabled event sources: syscall
# 2025-05-25T17:31:36+0000: Opening 'syscall' source with modern BPF probe.
# 2025-05-25T17:31:36+0000: One ring buffer every '2' CPUs.


##  Summary:  Falco generates event notifications and set up properly.
```
