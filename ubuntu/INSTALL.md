# Installing Kubernetes (1.19.2) on these three nodes (20.04)

## Step 1

Stand up VMs with `vagrant up`

## Step 2

Install Docker on all nodes:

`sudo bash /vagrant/ubuntu/install-docker-2.sh`

## Step 3

Install kubeadm,kubelet,kubectl on all nodes:

`sudo bash /vagrant/ubuntu/install-kube.sh`

## Step 4

Turn off swap on all nodes:

`sudo swapoff -a`

## Step 5

Initialize master with kubeadm

```
sudo kubeadm init --apiserver-advertise-address=192.168.56.2 --apiserver-cert-extra-sans=192.168.56.2 --pod-network-cidr=10.10.0.0/16
```

## Step 6

Install Calico on master

```
kubectl create -f https://docs.projectcalico.org/manifests/tigera-operator.yaml
kubectl create -f /vagrant/ubuntu/calico-install.yaml
```

Verify that master is Ready using `kubectl get node`

## Step 7

Join the 2 worker nodes to the cluster by copying the output of this command:

```
kubeadm token create --print-join-command
```

and running it with `sudo` on the worker nodes.
