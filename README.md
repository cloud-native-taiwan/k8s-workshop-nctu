# Kubernetes Workshop on CNTUG
The purpose of this workshop series is to get you on-boarded to Kuberentes experience. We will guide you to setup a cluster using `kubeadm`, and through tools(`kubectl`、`helm` or `kustomize`) to deploy applications.

- [Slide](https://drive.google.com/file/d/1iOsAa4HwXrNMfkkTJFA1mHt6glgpOYbL/view?usp=sharing)

## Requirements
* Minikube
    * [Linux](http://files.k2r2bai.com/minikube/bin/linux/amd64/minikube)
    * [Mac OS X](http://files.k2r2bai.com/minikube/bin/darwin/amd64/minikube)
    * [Windows](http://files.k2r2bai.com/minikube/bin/windows/amd64/minikube.exe)
    * [iso](http://files.k2r2bai.com/minikube/iso/minikube-v0.28.0.iso)

> Please download the minikube above URL.

* [Virtual Box](https://www.virtualbox.org/wiki/Downloads)
* [kubeclt](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Setup Cluster
First, Use `minikube` create the master node as following command:
```sh
$ minikube --profile k8s-m1 start
```

And then check the status via `kubectl`:
```sh
$ kubectl get no
NAME      STATUS    ROLES     AGE       VERSION
k8s-m1    Ready     master    2m        v1.11.0

$ kubectl -n kube-system get po -o wide
NAME                             READY     STATUS    RESTARTS   AGE       IP               NODE
coredns-78fcdf6894-cgwlj         1/1       Running   0          53s       10.244.0.3       k8s-m1
coredns-78fcdf6894-rwvt7         1/1       Running   0          53s       10.244.0.2       k8s-m1
etcd-k8s-m1                      1/1       Running   0          21s       192.168.99.100   k8s-m1
kube-addon-manager-k8s-m1        1/1       Running   0          23s       192.168.99.100   k8s-m1
kube-apiserver-k8s-m1            1/1       Running   0          7s        192.168.99.100   k8s-m1
kube-controller-manager-k8s-m1   1/1       Running   0          12s       192.168.99.100   k8s-m1
kube-flannel-ds-ftrv7            1/1       Running   0          51s       192.168.99.100   k8s-m1
kube-proxy-hxgfg                 1/1       Running   0          53s       192.168.99.100   k8s-m1
storage-provisioner              1/1       Running   0          51s       192.168.99.100   k8s-m1
```

Now, we will add new virtual machines as a Kubernetes node:
```sh
$ minikube --profile k8s-n1 start --node

# Get bootstrap token from master node
$ minikube --profile k8s-m1 ssh "sudo kubeadm token list"

# Use ssh go in virtual machine
$ minikube --profile k8s-n1 ssh 

# k8s-n1 vm
$ sudo su -
$ TOKEN=7rzqkm.1goumlnntalpxvw0
$ kubeadm join --token ${TOKEN} ${MASTER_IP}:8443 \
    --discovery-token-unsafe-skip-ca-verification \
    --ignore-preflight-errors=Swap \
    --ignore-preflight-errors=DirAvailable--etc-kubernetes-manifests
```

Finally, check the status via `kubectl`:
```sh
$ kubectl get no
NAME      STATUS    ROLES     AGE       VERSION
k8s-m1    Ready     master    6m        v1.11.0
k8s-n1    Ready     <none>    40s       v1.11.0
```
