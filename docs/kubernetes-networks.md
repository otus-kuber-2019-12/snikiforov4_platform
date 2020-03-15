# kubernetes-networks

### Useful links
[kubespy](https://github.com/pulumi/kubespy)<br> 

#### Work with Deployment strategies 
[Strategies](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)<br>

#### Scheduling
[Job Scheduling Algorithms in Linux Virtual Server](http://www.linuxvirtualserver.org/docs/scheduling.html)<br>
[Kubernetes Scheduling algorithms](https://github.com/kubernetes/kubernetes/blob/1cb3b5807ec37490b4582f22d991c043cc468195/pkg/proxy/apis/config/types.go#L185)<br> 

```bash
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

#### MetalLb
```bash
# Check installed parts of metallb 
kubectl --namespace metallb-system get all
```
