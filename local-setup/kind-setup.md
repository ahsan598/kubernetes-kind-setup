# 🚀 Kubernetes Local Setup using Kind (Kubernetes in Docker)

Kind = Kubernetes in Docker → Best for DevOps practice, CI testing & learning


### ⚙️ Prerequisites
| Requirement      | Status                  |
| ---------------- | ----------------------- |
| Docker Installed | Required                |
| RAM              | 4GB+ recommended        |
| OS Supported     | Windows / Linux / macOS |



### 🔧 Installtion Steps

**1. Install Kubectl**
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl
sudo mv kubectl /usr/local/bin/

kubectl version --client
```

**2. Install Kind**
```sh
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64

chmod +x kind
sudo mv kind /usr/local/bin/kind

kind version
```

**3. Create Kubernetes Cluster**
```sh
kind create cluster --name dev-cluster

#Test Cluster
kubectl cluster-info
kubectl get nodes
```

**4. Set Context for Namespace**
```sh
kubectl config set-context kind-dev-cluster --namespace=dev

kubectl config get-contexts
```


### 🧪 Kubernetes Basic Operations

**1. Deployment Create/Delete**
```sh
kubectl create deployment nginx --image=nginx -n dev
kubectl get pods -n dev

kubectl delete deployment nginx -n dev
```

**2. Expose using NodePort**
```sh
kubectl expose deployment nginx --type=NodePort --name=nginx-svc --port=80 -n dev
kubectl get svc nginx-svc -n dev
```
**Access Service by port forwarding**
```sh
kubectl port-forward svc/nginx-svc 8082:80

http://localhost:<nodePort>
```

**3. Scaling Pods**
```sh
kubectl scale deployment nginx --replicas=5 -n dev
kubectl get pods -o wide -n dev
```

**4. Rollout & Undo Updates**
```sh
kubectl set image deployment/nginx nginx=nginx:1.25 --record -n dev
kubectl rollout undo deployment/nginx -n dev
```

### 🔧 Kind Cluster Management

**1. Pause cluster (Memory free → restart later possible)**
```sh
# Export config before stopping (optional)
kind export kubeconfig --name dev-cluster

docker stop kind-control-plane
```

**2. Stop multiple node cluster**
```sh
docker ps | grep kind
docker stop <all-kind-node-containers>
```

**3. Resume cluster**
```sh
docker start <container_name>
```

**4.  Completely delete cluster (fresh reset)**
```sh
kind delete cluster --name dev-cluster
```


### 🧭 Quick Decision Guide

| Task                 | What to do                                       |
| -------------------- | ------------------------------------------------ |
| Remove app only      | `kubectl delete deployment` / scale `replicas=0` |
| Free RAM temporarily | `docker stop kind-*`                             |
| Fresh start cluster  | `kind delete cluster`                            |

