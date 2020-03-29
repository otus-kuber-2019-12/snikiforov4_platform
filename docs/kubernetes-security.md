# kubernetes-security 

### Useful links
[Kubernetes Security Guide](https://sysdig.com/wp-content/uploads/2019/01/kubernetes-security-guide.pdf)<br>
[Пользователи и авторизация RBAC в Kubernetes](https://habr.com/ru/company/flant/blog/470503/)<br>
[Using RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)<br>

#### Task 1
- Создать Service Account bob, дать ему роль admin в рамках всего кластера
- Создать Service Account dave без доступа к кластеру

#### Task 2
- Создать Namespace prometheus
- Создать Service Account carol в этом Namespace. 
- Дать всем Service Account в Namespace prometheus возможность делать get, list, watch в отношении Pods всего кластера

#### Task 3
- Создать Namespace dev
- Создать Service Account jane в Namespace dev
- Дать jane роль admin в рамках Namespace dev
- Создать Service Account ken в Namespace dev
- Дать ken роль view в рамках Namespace dev
