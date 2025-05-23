
### Control Plane Node Configuration

#### Step 1: Setup containerd
```sh
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
```
```sh
modprobe overlay
modprobe br_netfilter
```
```sh
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF
```
```sh
sysctl --system
```
```sh
apt-get install -y containerd
mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml
```
```sh
nano /etc/containerd/config.toml
```
  --> `SystemdCgroup = true`

```sh
systemctl restart containerd
```

#### Step 2: Kernel Parameter Configuration
```sh
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
```
```sh
sudo sysctl --system
```

#### Step 3: Configuring Repo and Installation
```sh
sudo apt-get update
apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.32/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.32/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```sh
sudo apt-get update
 apt-get install -y kubelet=1.32.0-1.1 kubeadm=1.32.0-1.1 kubectl=1.32.0-1.1 cri-tools=1.32.0-1.1
sudo apt-mark hold kubelet kubeadm kubectl
systemctl enable --now kubelet
```

#### Step 4 - Initialize Cluster with kubeadm:
```sh
kubeadm init --pod-network-cidr=192.168.0.0/16 --kubernetes-version=1.32.0
```
```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
#### Step 5 - Remove the Taint:
```sh
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```
#### Step 6 - Install Network Addon (Calico):
```sh
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/tigera-operator.yaml

kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.29.1/manifests/custom-resources.yaml
```
####  7 - Verification:
```sh
kubectl get nodes
kubectl run nginx --image=nginx
kubectl get pods
```

### Worker Node Configuration

Run Step 1 to Step 3 from Master Node configuration in worker node as well

Use the `kubeadm join` command that was generated in your Control Plane Node server. The below command is just for reference.

```sh
kubeadm join 209.38.120.248:6443 --token 9vxoc8.cji5a4o82sd6lkqa \
        --discovery-token-ca-cert-hash sha256:1818dc0a5bad05b378dd3dcec2c048fd798e8f6ff69b396db4f5352b63414baf
```
Run the following command in Mater node to ensure that worker node is in Ready status.

```sh
kubectl get nodes
```

---

Next: [16. Revising Taints and Tolerations](taint-toleration.md) <br>
Previous: [14. Implementing Auditing](audit-logs.md)
