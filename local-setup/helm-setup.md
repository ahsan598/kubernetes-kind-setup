# 🧭 Helm Guide for Kubernetes (With Kind Cluster)

### 📌 What is Helm?

Helm = Kubernetes package manager

| Without Helm                | With Helm                        |
| --------------------------- | -------------------------------- |
| Multiple YAML files         | Single chart package (.tgz)      |
| Manual deployment & updates | Versions, upgrade, rollback      |
| Hard to re-use              | Template-based, reusable configs |


### 🛠 Install Helm

**1. Linux/macOS**
```sh
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

helm version
```

**2. Windows (Chocolatey)**
```sh
choco install kubernetes-helm

helm version
```

### Helm Essential Commands

| Purpose            | Command                               |
| ------------------ | ------------------------------------- |
| Search charts      | `helm search hub nginx`               |
| Install chart      | `helm install my-nginx bitnami/nginx` |
| List installations | `helm list`                           |
| Upgrade release    | `helm upgrade my-nginx bitnami/nginx` |
| Rollback version   | `helm rollback my-nginx 1`            |
| Uninstall          | `helm uninstall my-nginx`             |


### Add Helm Repo

**1. Helm Repo Add (Required before install chart)**
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update
```

**2. Install Nginx using Helm**
```sh
helm install webserver bitnami/nginx -n dev --create-namespace

kubectl get pods -n dev
kubectl get svc -n dev
```

Expose via NodePort (if not exposed)
```sh
kubectl expose deployment webserver-nginx --type=NodePort --name=nginx-svc -n dev
```

**3. Upgrade & Rollback**
- Update version or values:
```sh
helm upgrade webserver bitnami/nginx --set service.type=NodePort -n dev
```

- Rollback to previous revision
```sh
helm history webserver -n dev

helm rollback webserver 1 -n dev
```

### Custom Charts

1. Create Your Own Helm Chart
```sh
helm create myapp
```

Folder created:
```rust
myapp/
 ├ charts/
 ├ templates/   --> YAML templates here
 ├ values.yaml  --> Your override configs
 └ Chart.yaml
```

2. Deploy Your Custom Chart
```sh
helm install myapp-release ./myapp -n dev

kubectl get all -n dev
```

Update changes → deploy again
```sh
helm upgrade myapp-release ./myapp -n dev
```

3. Change Parameters with `values.yaml`
```yml
replicaCount: 4
image:
  repository: nginx
  tag: "1.25"
service:
  type: NodePort
  port: 80
```

Deploy with values:
```sh
helm upgrade myapp-release ./myapp -f values.yaml -n dev
```

4. Package & Share Your Helm Chart
- Create distributable `.tgz`
```sh
helm package myapp/
```

- Repo host/test local chart
```sh
helm install local-app myapp-0.1.0.tgz -n dev
```


### Summary Flow

| Step            | Command                   |
| --------------- | ------------------------- |
| Create Chart    | `helm create app`         |
| Edit values     | `values.yaml` Modify      |
| Deploy          | `helm install app ./app`  |
| Upgrade changes | `helm upgrade app ./app`  |
| Rollback        | `helm rollback app <REV>` |
