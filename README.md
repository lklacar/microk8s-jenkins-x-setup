# microk8s-jenkins-x-setup

- Installation
```bash
# Install microk8s
sudo snap install microk8s --classic

# Enable microk8s features
microk8s.enable dns storage ingress

# Install jx
mkdir -p ~/.jx/bin
curl -L https://github.com/jenkins-x/jx/releases/download/v1.3.566/jx-linux-amd64.tar.gz | tar xzv -C ~/.jx/bin
export PATH=$PATH:~/.jx/bin
echo 'export PATH=$PATH:~/.jx/bin' >> ~/.bashrc

# Install kubectl, because microk8s.kubectl uses wrong kube config or you can just use kubectl on your local machine, not a cluster.
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl

# Setup the correct kube confug
microk8s.config > ~/.kube/config

# Check if kubectl works
kubectl get all --all-namespaces
```

- Create StorageClass
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
```

```bash
kubectl create -f storage.yaml
jx install --provider=kubernetes --external-ip=<IP> --domain=devlab.rs --on-premise
```
```bash
sudo iptables -P FORWARD ACCEPT
```

```bash
jx install --provider=kubernetes --external-ip <IP> \
--ingress-service=default-http-backend \
--ingress-deployment=default-http-backend \
--ingress-namespace=default \
--on-premise \
--domain=devlab.rs
```


- Remove microk8s
```bash
microk8s.reset
sudo snap remove microk8s
```

- Useful commands. No need to run these during the installation.

```bash 
# alias microk8s kubectl - DON'T DO THIS
sudo snap alias microk8s.kubectl kubectl
```

[Jenkins X Docs](https://jenkins-x.io/getting-started/install-on-cluster/#installing-jenkins-x-on-premise)
