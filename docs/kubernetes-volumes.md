# kubernetes-volumes

### Useful links
[MinIO](https://min.io/)<br>
[GitHub MinIO MC command line tool](https://github.com/minio/mc)<br>
[K8s secrets](https://kubernetes.io/docs/concepts/configuration/secret/)<br>


#### Apply MinIO
```bash
kubectl apply -f minio-statefulset.yaml
kubectl apply -f minio-headlessservice.yaml

# Check what has done use following commands
kubectl get statefulsets
kubectl get pods
kubectl get pvc
kubectl get pv
kubectl describe <resource> <resource_name>
```
