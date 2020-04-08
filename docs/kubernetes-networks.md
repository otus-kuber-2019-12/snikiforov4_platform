# kubernetes-networks

### Useful links
[kubespy](https://github.com/pulumi/kubespy)<br> 
[IPVS](https://github.com/kubernetes/kubernetes/blob/master/pkg/proxy/ipvs/README.md)<br> 

#### Work with Deployment strategies 
[Strategies](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)<br>

#### Scheduling
[Job Scheduling Algorithms in Linux Virtual Server](http://www.linuxvirtualserver.org/docs/scheduling.html)<br>
[Kubernetes Scheduling algorithms](https://github.com/kubernetes/kubernetes/blob/1cb3b5807ec37490b4582f22d991c043cc468195/pkg/proxy/apis/config/types.go#L185)<br> 

```bash
# After applying deployment it's state could be checked using following methods
# "trace" changes over "web" deployment
kubespy trace deploy web
# Watch events
kubectl get events --watch
``` 

Play with `maxSurge` & `maxUnavailable`:<br>
1. `maxUnavailable: 0 maxSurge: 0`
Выдаст ошибку.
2. `maxUnavailable: 100% maxSurge: 0`
Остановит существующие контейнеры и создаст новые. 
3. `maxUnavailable: 0 maxSurge: 100%`
Запустит новые контейнеры и после остановит уже существующие контейнеры. 
4. `maxUnavailable: 100% maxSurge: 100%`
Остановка старых и после запуск новых контейнеров. (аналогично кейсу №2)

#### ClusterIP

```bash
# Creating CLusterIP service
kubectl apply -f web-svc-cip.yaml

# Check results 
kubectl get services
curl http://<CLUSTER-IP>/index.html 
# работает!
ping <CLUSTER-IP> 
# пинга нет
arp -an, ip addr show 
# нигде нет ClusterIP
iptables --list -nv -t nat
```

##### Enable IPVS
```bash
minikube dashboard
# Set namespace to kube-system
# Configs and Storage/Config Maps
# Set mode: "ipvs"
```
Create file /tmp/iptables.cleanup with content
```bash
*nat
-A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE
COMMIT
*filter
COMMIT
*mangle
COMMIT
```
```bash
# Apply config 
iptables-restore /tmp/iptables.cleanup
# Check results 
iptables --list -nv -t nat
```

##### Who to check IPVS configuration 
```bash
# Inside minikube
toolbox
dnf install -y ipvsadm && dnf clean all
# Check ours ip
ipvsadm --list -n
```

#### LoadBalancer
```bash
# Install MetalLb
kubectl apply -f metallb.yml
# Check installed parts of metallb 
kubectl --namespace metallb-system get all
# Create ConfigMap
kubectl apply -f metallb-config.yaml
# Create LoadBalancer service 
kubectl apply -f web-svc-lb.yaml

# Add routes on host machine 
sudo route add 172.17.255.0/24 $(minikube ip) 
```

#### Ingress

```bash
# Install k8s ingress
kubectl apply -f ingress-nginx.yaml
# Create LoadBalancer service
kubectl apply -f nginx-lb.yaml
kubectl apply -f web-svc-headless.yaml
kubectl apply -f web-ingress.yaml
# check ingress works 
kubectl describe ingress/web
```
