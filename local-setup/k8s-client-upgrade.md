# Kubernetes Client Tools Upgrade Guide (Kind | Helm | Kubectl)

This guide helps you update your local Kubernetes toolchain to the latest versions.


**1. Update Kind**
```sh
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.30.0/kind-linux-amd64

chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

kind version
```

**2. Update Helm**
```sh
curl -fsSL https://raw.githubuserconcom/helm/helm/main/scripts/get-helm-4 | bash

helm version
```

**3. Update Kubectl**
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl

kubectl version --client
```
