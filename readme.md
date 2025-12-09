# 🚀 Kubernetes Local Setup with KIND (Kubernetes in Docker)

KIND = Run Kubernetes inside Docker containers → Ideal for local DevOps practice, CI, and learning.


### ⚙️ Prerequisites
| **Tools**        | **Status**   |
| ---------------- | ------------ |
| Docker           | Required     |
| Kubectl          | Required     |


### 🔧 Installtion Steps

**1. Install Kubectl CLI**
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl
sudo mv kubectl /usr/local/bin/

kubectl version --client
```

**2. Install Kind**
```sh
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64

chmod +x kind
sudo mv kind /usr/local/bin/kind

kind version
```

### 🚀 Create a Kubernetes Cluster
**1. Default single-node cluster**
```sh
kind create cluster --name <cluster-name>           # name: dev-cluster

# Verify
kubectl cluster-info

kubectl cluster-info --context kind-dev-cluster
kubectl get nodes
```

**2. Set default context (use cluster name you want to switch)**
```sh
kubectl config get-contexts

kubectl config set-context kind-dev-cluster             # --namespace=dev
```

**3.  Completely delete cluster (fresh reset)**
```sh
kind delete cluster --name <cluster-name>
```

### 🧩 Multi Node Cluster
**1. KIND config file `kind-config.yml`**
```yml
# three node (two workers) cluster config
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

**2. Create cluster with config**
```sh
kind create cluster --name <cluster-name> --config kind-config.yaml

Ex: kind create cluster --name dev-cluster2 --config kind-config.yaml
```

**3. Set context to dev-cluster2**
```sh
kubectl config get-contexts

kubectl config set-context kind-dev-cluster2
```

**4.  Completely delete cluster (fresh reset)**
```sh
kind delete cluster --name <cluster-name>
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

---

### Reference
- [Kind Docs](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker Install](https://docs.docker.com/engine/install/)