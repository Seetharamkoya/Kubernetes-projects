# Kubernetes-projects
summary of notes and hands on projects from Kubernetes Enterprise Guide

## Kubernetes Traditional cluster setup
> Step 1: Create k8s cluster in 3 node lab setup (1 master & 2 worker node)



create ubuntu 18.0 LTS machines like below

Server is going to be the kubenetes master
node1,node2 going to be Worker nodes


Commands to install the "kUBERNETES CLUSTER"



Install Docker Engine (run all the nodes)
---------------------
> apt-get update && apt-get install -y apt-transport-https curl
> curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

> cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF


>apt-get update

>apt-get install -y kubelet kubeadm kubectl 

> apt-mark hold kubelet kubeadm kubectl

Install Docker packages (run all the nodes)



---------------------------

> apt-get update && apt-get install apt-transport-https ca-certificates curl software-properties-common -y

> curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

> add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

> apt-get update && apt-get install docker-ce=18.06.2~ce~3-0~ubuntu -y

> cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

> mkdir -p /etc/systemd/system/docker.service.d

Restart docker.
> systemctl daemon-reload
> systemctl restart docker
> systemctl enable docker

*********************************************************************************
We need to run this below command in only Server (Master node).coz we are going to make master machine as kube master

> kubeadm init --apiserver-advertise-address $(hostname -i) --pod-network-cidr=192.168.0.0/16
  
> mkdir -p $HOME/.kube
> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
> sudo chown $(id -u):$(id -g) $HOME/.kube/config
> export kubever=$(kubectl version | base64 | tr -d '\n')

https://www.weave.works/docs/net/latest/kubernetes/kube-addon/ (kube proxy addons installation)

kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"


verify:

kubectl get nodes

now from node1 and node2 join to the k8s cluster 

kubectl get nodes (this command output must show all nodes are ready status)

**********************************************************************************************

watch -n 1 kubectl get nodes
  
  Steps in Master
  
  ![image](https://user-images.githubusercontent.com/38424194/155899036-90f73562-7d56-4936-8da6-6d3e97ba87b5.png)
  
  steps in nodes
  
  ![image](https://user-images.githubusercontent.com/38424194/155899102-1d02ae95-3040-4f50-ada3-9479f8fad8c5.png)

