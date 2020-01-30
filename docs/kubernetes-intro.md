# kubernetes-intro 

### Useful links
Инструкци по установке [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)<br> 
Настройка [автокомплитов](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete)<br>
Инструкция по устновке [minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/)<br>
Инструкция по установке [k9s](https://github.com/derailed/k9s)</br>

```bash
# Запуск локальной виртуальной машины с кластером
minikube start
# Проверка состояния кластера 
kubectl cluster-info
```

```bash
# Просмотр всех контейнеров внутри minikube
minikube ssh
docker ps

# Эти же компоненты, но уже в виде pod
kubectl get pods -n kube-system

# Проверка кластера 
kubectl get componentstatuses
kubectl get cs
```


### WEB-SERVER

#### Особенности сервиса:
1) web-сервер на порту 8000 
2) работающий с UID 1001
3) Отдающий содержимое директории /app внутри контейнера

#### Файлы для работы с сервисом: 
- web/Dockerfile 
- web/build.sh - билд докер образ с тэгом `web-static-content`
- web-pod.yaml - манифест для запуска сервиса в кластере

#### Описание процесса паблиша образа в DockerHUB
```bash
# Publish image to Docker Hub
DOCKER_USER=snikiforov4
DOCKER_IMAGE_TAG=1.0.2
docker login -u ${DOCKER_USER}
docker tag web-static-content ${DOCKER_USER}/web-static-content:${DOCKER_IMAGE_TAG}
docker push ${DOCKER_USER}/web-static-content:${DOCKER_IMAGE_TAG}
```

#### Запуск сервиса
```bash
# Запустить сервис в кластере
kubectl apply -f web-pod.yaml
# Получить информаци обо всех запущенных pod-ах
kubectl get pods
# Проверить статсу запушенного pod 
kubectl describe pod web

# Пробросить порт наружу 
kubectl port-forward --address 0.0.0.0 pod/web 8000:8000
```

#### Проверка работы сервиса
```text
При открытии http://localhost:8000/ должна отобразиться стартовая страница
```

Easy to use Kubernetes port forwarding manager [kube-forwarder](https://kube-forwarder.pixelpoint.io/)</br>
