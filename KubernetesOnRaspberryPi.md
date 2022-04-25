# Kubernetes on RaspberryPi

## Network

- Top : k8s1
  - MAC : DC:A6:32:C5:A1:88
  - Local IP : 192.168.1.28
  - Control
- Middle : k8s2
  - MAC : DC:A6:32:C5:A1:B5
  - Local IO : 192.168.1.21
  - Worker
- Bottom : k8s3
  - MAC : DC:A6:32:C5:A2:09
  - Local IP : 192.168.1.27
  - Worker

## SSH Commands

```sh
ssh ubuntu@192.168.1.28 -i ./id_ed25519
ssh ubuntu@192.168.1.21 -i ./id_ed25519
ssh ubuntu@192.168.1.27 -i ./id_ed25519
```

## Setup for each node

See docs [https://github.com/CyberAgentHack/home-kubernetes-2020/tree/master/how-to-create-cluster-logical-kubeadm]

```sh
hostnamectl set-hostname k8s1 # for 12
hostnamectl set-hostname k8s2 # for 11
hostnamectl set-hostname k8s3 # for 14
```

```sh
cat << _EOF_ | sudo tee -a /etc/hosts
192.168.11.12  k8s1
192.168.11.11  k8s2
192.168.11.14  k8s3
_EOF_
```

```sh
# カーネルパラメータの変更
cat << _EOF_ | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
_EOF_

sudo sysctl --system
sudo systemctl reboot -i
```

```sh
sudo apt-get update -y && sudo apt-get -y install docker-ce=5:19.03.12~3-0~ubuntu-focal docker-ce-cli=5:19.03.12~3-0~ubuntu-focal containerd.io=1.2.13-2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat << _EOF_ | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
_EOF_

sudo apt-get update -y && sudo apt-get install -y kubelet=1.19.2-00 kubeadm=1.19.2-00 kubectl=1.19.2-00
sudo apt-mark hold kubelet kubeadm kubectl
```

### On Master

```sh
kubeadm init --pod-network-cidr=10.244.0.0/16 --control-plane-endpoint=k8s1 --apiserver-cert-extra-sans=k8s1
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl label node k8s2 node-role.kubernetes.io/worker=worker
kubectl label node k8s3 node-role.kubernetes.io/worker=worker
```

### On worker nodes

```sh
sudo kubeadm join k8s1:6443 --token hyokru.cdcdepvu6e44xlz5 \
    --discovery-token-ca-cert-hash sha256:b0ad3134c45ebd649b1496067237ac7bd0be06cb0137746ff8f922b0962cc450
```

### Remote Access to The Cluster

- Register host name and ip pairs to `C:\Windows\System32\drivers\etc`
- Download config to local
- Merge config files accordingly

```powershell
gsudo -w notepad c:\\Windows\system32\drivers\etc\hosts
```

```sh
scp ubuntu@k8s1:~/.kube/config .\.kube\
```

## Addons

[link](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

## Renew certificates on Kubernetes

[link](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/)

```powershell
journalctl -xeu kubelet
sudo kubeadm certs check-expiration
kubeadm certs renew all
```

```txt
/etc/kubernetes/kubelet.conf is expired: 2021-10-13 08:35:35 +0000 UTC
unable to load bootstrap kubeconfig: stat /etc/kubernetes/bootstrap-kubelet.conf: no such file or directory"
```

```txt
ubuntu@k8s1:~$ sudo ls -la /etc/kubernetes/
total 48
drwxr-xr-x   5 root root 4096 Dec 13  2020 .
drwxr-xr-x 105 root root 4096 Mar 31 06:19 ..
-rw-------   1 root root 5584 Mar 29 23:05 admin.conf
-rw-------   1 root root 5629 Mar 29 23:05 controller-manager.conf
-rw-------   1 root root 1900 Oct 13  2020 kubelet.conf
drwxr-xr-x   2 root root 4096 Dec 20  2020 manifests
drwxr-xr-x   3 root root 4096 Oct 13  2020 pki
-rw-------   1 root root 5573 Mar 29 23:05 scheduler.conf
drwx------   6 root root 4096 Dec 20  2020 tmp
ubuntu@k8s1:~$ sudo ls -la /etc/kubernetes/pki/
total 68
drwxr-xr-x 3 root root 4096 Oct 13  2020 .
drwxr-xr-x 5 root root 4096 Dec 13  2020 ..
-rw-r--r-- 1 root root 1155 Mar 29 23:05 apiserver-etcd-client.crt
-rw------- 1 root root 1675 Mar 29 23:05 apiserver-etcd-client.key
-rw-r--r-- 1 root root 1164 Mar 29 23:05 apiserver-kubelet-client.crt
-rw------- 1 root root 1679 Mar 29 23:05 apiserver-kubelet-client.key
-rw-r--r-- 1 root root 1277 Mar 29 23:05 apiserver.crt
-rw------- 1 root root 1679 Mar 29 23:05 apiserver.key
-rw-r--r-- 1 root root 1066 Oct 13  2020 ca.crt
-rw------- 1 root root 1675 Oct 13  2020 ca.key
drwxr-xr-x 2 root root 4096 Oct 13  2020 etcd
-rw-r--r-- 1 root root 1078 Oct 13  2020 front-proxy-ca.crt
-rw------- 1 root root 1679 Oct 13  2020 front-proxy-ca.key
-rw-r--r-- 1 root root 1119 Mar 29 23:05 front-proxy-client.crt
-rw------- 1 root root 1675 Mar 29 23:05 front-proxy-client.key
-rw------- 1 root root 1675 Oct 13  2020 sa.key
-rw------- 1 root root  451 Oct 13  2020 sa.pub
```

```shell
sudo kubeadm certs generate-csr --kubeconfig-dir ~/cert --cert-dir ~/cert/pki
```

## MetalLB

[installation](https://metallb.universe.tf/installation/)

## Mount drive and assign as PV

[Qiita](https://qiita.com/reireias/items/76a22b938f1d3f829f8d)