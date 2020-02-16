# kubernetes-controllers 

### Useful links
kind [Quick Start](https://kind.sigs.k8s.io/docs/user/quick-start/#quick-start)<br> 
JSONPath [Support](https://kubernetes.io/docs/reference/kubectl/jsonpath/)<br>

Kubernetes Controllers<br>
[ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)<br> 
[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) / [Strategies](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)<br> 
[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)<br> 
[Liveness and Rediness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)<br> 
[Taints and Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)<br>

#### Work with kind
```bash
# create cluster
kind create cluster --config kind-config.yaml

# check cluster nodes up and running
kubectl get nodes
``` 


#### ReplicaSet & Deployment
```bash
# ad-hoc command to scale ReplicaSet 
kubectl scale replicaset frontend --replicas=3
# To check ReplicaSet will recover _missing_ pods, remove pods and check 
kubectl delete pods -l app=frontend | kubectl get pods -l app=frontend -w

# Check image used inside ReplicaSet 
kubectl get replicaset frontend -o=jsonpath='{.spec.template.spec.containers[0].image}'
# Check actual version of running pods 
kubectl get pods -l app=frontend -o=jsonpath='{.items[0:3].spec.containers[0].image}'

# After applying new deployment to view deployment rolling out history
kubectl rollout history deployment paymentservice
# Get back to previous deployment version 
kubectl rollout undo deployment paymentservice --to-revision=1 | kubectl get rs -l
app=paymentservice -w
``` 
To check any of recourse use:
```bash
kubectl get rs, deployment, pods
```  

#### DaemonSet
After applying manifest to get metrics from any pod, port 9100 should be forwarded.  
```bash
kubectl port-forward -n monitoring <any pod inside DaemonSet> 9100:9100  
```
To get metrics:
```bash
curl localhost:9100/metrics
```