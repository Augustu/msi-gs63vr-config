### kubernetes

#### install

```bash
sudo kubeadm init \
	--kubernetes-version=v1.20.2 \
	--image-repository registry.aliyuncs.com/google_containers \
	--ignore-preflight-errors=all \
	--apiserver-advertise-address=118.31.14.196 \
	--apiserver-cert-extra-sans 0.hangzhou.k8s.didida.top \
	--pod-network-cidr=10.10.0.0/16 \
	--v=5

# To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

# Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

# Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.0.10:6443 --token stc549.yndw02rmd9758kg4 \
  --discovery-token-ca-cert-hash sha256:40de8ced84caaaee2b8edc91f332387848c682a4a51228985ba4cf0bd4702f22
```

#### reduce components resource require

```bash
cd /etc/kubernetes/manifests

ls
# etcd.yaml  kube-apiserver.yaml  kube-controller-manager.yaml  kube-scheduler.yaml

# auto enable after edit yaml files
```



