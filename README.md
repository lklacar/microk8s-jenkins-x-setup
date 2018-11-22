# microk8s-jenkins-x-setup
```bash
# Install microk8s
sudo snap install microk8s --classic

# Install jx
mkdir -p ~/.jx/bin
curl -L https://github.com/jenkins-x/jx/releases/download/v1.3.585/jx-linux-amd64.tar.gz | tar xzv -C ~/.jx/bin
export PATH=$PATH:~/.jx/bin
echo 'export PATH=$PATH:~/.jx/bin' >> ~/.bashrc

# Install kubectl, because microk8s.kubectl uses wrokg kube config.
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl

# Setup the correct kube confug
microk8s.config > ~/.kube/config

# Check if kubectl works
kubectl get all --all-namespaces

# Install Jenkins X on microk8s cluster
jx install --provider=kubernetes --on-premise
```

- Remove microk8s
```bash
microk8s.reset
sudo snap remove microk8s
```

- Util 

```bash 
# alias microk8s kubectl - DON'T DO THIS
sudo snap alias microk8s.kubectl kubectl

# If neededm, but jenkins x should create everything
microk8s.enable dns dashboard storage ingress
```
