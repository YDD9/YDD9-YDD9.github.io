---
layout: post
title:  "Dokcer Kubernetes"
date:   2017-12-21 14:58:39 +0100
comments: true 
categories: Docker Kubernetes
---

- [Simple example using Docker to run Python](#simple-example-using-docker-to-run-python)
- [minikube install](#minikube-install)
- [kubeadm install](#kubeadm-install)   
        - [try solutions to disable swap:](#try-solutions-to-disable-swap)   
        - [Install Flannel for pod networking](#install-flannel-for-pod-networking)    
- [trouble shoot:](#trouble-shoot)   


# Simple example using Docker to run Python  
https://www.civisanalytics.com/blog/using-docker-to-run-python/  
change the Dockerfile based on the help https://codefresh.io/docker-guides/build-docker-image-dockerfiles/
https://github.com/YDD9/docker-python-hello-kubernetes is my repo for total setup and learning with a web app.

Dockerfile
a file without extension
```
FROM python 
ADD "./Birthday-Problem/birthday.py" "./src/birthday.py"
CMD ["python", "/src/birthday.py"]
```
To build from current dir ./ and specify a name
```
$ sudo docker build ./ -t ydd9/python-birthday
Successfully built ydd9/python-birthday:latest
$ sudo docker build ./
Successfully built da3bf65a40da
```

To run the app in docker, just use command `docker run <built image>`  
```
sudo docker run ydd9/python-birthday
# but below works as well
sudo docker run ydd9/python-birthday python /src/birthday.py
```

# minikube install
minikube will start a Kubernetes cluster on your local or VM environment and you can use it for learning kubernetes commands.   
Install minikube https://github.com/kubernetes/minikube#using-rkt-container-engine
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /opt/bin/

curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl
```

https://stackoverflow.com/questions/37810293/coreos-read-only-file-system  
In CoreOS the /usr partition is read-only by design, so /usr/local/bin/ will be read-only too.  use /opt/ for this purpose. 
Do `mkdir -p /opt/bin; mv ./kubectl /opt/bin/kubectl`, minikube still can't run as it always goes to /usr path to write.


# kubeadm install
kubeadm is designed to help you create Kubernetes cluster env on VMs or cloud hosted systems. It provides the minimum setup of Kubernetes cluster. Now I'm using Debian9.3 stretch in VirtualBox5.2

Environment checking: your nodes' MAC address and network adaptor.
In case you need to add route in your PC, you can check https://www.cyberciti.biz/faq/linux-route-add/
```
$ apt-get install net-tools
$ route -n

# temporarily add
# Route all traffic via 192.168.1.254 gateway connected via eth0 network interface:
$ route add default gw 192.168.1.254 eth0

# permenatly add
$ nano /etc/rc.local
# append your route:  /sbin/ip route add 192.168.1.0/24 dev eth0
```

First, install Docker-CE in Debian9.3 stretch if it is not installed. You can check by `sudo docker run hello-world`</br>
Follow official installation and check the part for Debian stretch version, the link is     https://docs.docker.com/engine/installation/linux/docker-ce/debian/#set-up-the-repository</br>

Pay attention to the Docker version, do not install latest Docker-CE which Kubernetes does not support, install Docker-CE <= 17.03 for now (Dec 2017)    

Post-Docker installation</br>
To avoid always do `sudo docker ...`, we need to put docker group into sudo users, [manual](https://docs.docker.com/engine/installation/linux/linux-postinstall/#manage-docker-as-a-non-root-user)
```
 sudo groupadd docker
 sudo usermod -aG docker $USER
```


Then install kubeadm https://kubernetes.io/docs/setup/independent/install-kubeadm/ 
```
apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
```

On your desired Kubernetes cluster master, you can init a cluster   
In case of error, go to check solution https://github.com/kubernetes/kubernetes/issues/53333  
```
 [ERROR Swap]: running with swap on is not supported. Please disable swap
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-p
```

### try solutions to disable swap:
```
# solution one, needs to do again after reboot
iptables -F
swapoff -a
free -m
kubeadm reset

kubeadm init
```

```
# solution two, needs to do again after reboot
# delete the nodes if you have.
kubeadm reset
# add "Environment="KUBELET_EXTRA_ARGS=--fail-swap-on=false"" to /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
systemctl daemon-reload
systemctl restart kubelet

kubeadm init
```

```
# solution three, PERMENANT
# https://serverfault.com/questions/684771/best-way-to-disable-swap-in-linux

# immediately disable swap.
$ swapoff -a 

# remove or comment any swap entry in /etc/fstab
$ nano /etc/fstab

$ reboot 
# If the swap is gone, good. If not, for some reason, you had to remove the swap partition. 
# Repeat above two septs, then use fdisk or parted to remove the (now unused) swap partition. Reboot
```

Once init successfully finish, copy the message and you will need to use it to add other nodes into this cluster.
```
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join --token cf016f.b7fd1ad86da928cf 10.0.2.15:6443 --discovery-token-ca-cert-hash sha256:e7bd6e7561fda671554625787e8a71be7818a4f071d91b9bf78ec815556b8023
  
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

To verify if this kubernetes cluster has the cluster master running
```
kubectl -n kube-public get cm
```

Fail to add node into Kubernetes cluster
In another VM login as root and add it into cluster
```
$ kubeadm join --token cf016f.b7fd1ad86da928cf 10.0.2.15:6443 --discovery-token-ca-cert-hash sha256:e7bd6e7561fda671554625787e8a71be7818a4f071d91b9bf78ec815556b8023

Failed to request cluster info, will try again: [Get https://10.0.2.15:6443/api/v1/namespaces/kube-public/configmaps/cluster-info: dial tcp 10.0.2.15:6443: getsockopt: connection refused]
```
As we are using VMs, and the network config is NAT, so the master default network address and node address will be the same 10.0.2.15
This can't work for node to discover cluster master. https://github.com/kubernetes/kubernetes/issues/33562  
```
$ kubeadm init --help
Usage:
  kubeadm init [flags]

Flags:
      --apiserver-advertise-address string      The IP address the API Server will advertise it's listening on. Specify '0.0.0.0' to use the address of the default network interface.

      --apiserver-bind-port int32               Port for the API Server to bind to. (default 6443)

$ kubeadm init --apiserver-advertise-address '10.244.0.111' --pod-network-cidr=10.244.0.0/16
[init]   

...
```
The init process take 1-2 minutes is okay, if longer means it gets stuck like my case. The only explaination would be the IPs and Virtualbox network config as NAT. It's not a good idea to use NAT(need port forwarding to access host) or Host-only(no internet access), the best is to use Bridged network config for all VMs. Login to each VM and type `ip a` to find their IP 192.168.0.39-40 for host to connect.
```
$ kubeadm init --apiserver-advertise-address 192.168.0.39
```
https://www.virtualbox.org/manual/ch06.html   

Table 6.1. Overview

|  | VM ↔ Host | VM1 ↔ VM2 | VM → Internet | VM ← Internet |
|:--:|-----------|-----------|---------------|--------------|
|Host-only | + | + | – | –
|Internal | – | + | – | – |
|Bridged | + | + | + | + |
|NAT | – | – | + | Port forwarding |
|NAT Network | – | + | + | Port forwarding |


### Install Flannel for pod networking   
```
# You must pass flag to init: kubeadm init has already flag --pod-network-cidr=<ip range>
$ kubeadm init has already flag --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=192.168.0.39

# double check if the given commands after cluster setup is executed !!!!
# Set /proc/sys/net/bridge/bridge-nf-call-iptables to 1 by running
$ sysctl net.bridge.bridge-nf-call-iptables=1 
# to pass bridged IPv4 # traffic to iptables’ chains. This is a requirement for some CNI plugins to work.

$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml
The connection to the server 192.168.0.39:6443 was refused - did you specify the right host or port?
```

This connection error is difficult to find, first check network.  
```
# ip of the machine
$ hostname -i
# other IPs
$ hostname -I
# list out all IPs
$ ip a
# compare output IPs, they should be consistent, otherwise edit /etc/hosts file as below sample:
# 127.0.0.1       localhost
# 127.0.1.1       debian01
# 192.168.0.39    kubemaster.test.com kubemaster
# 192.168.0.40    kubeslave1.test.com kubeslave1

$ apt-get install dnsutils
$ nslookup kubemaster.test.com
Server:         192.168.0.1
Address:        192.168.0.1#53

Non-authoritative answer:
Name:   kuebmaster.test.com
Address: 69.172.200.109


$ kubeadm init --apiserver-advertise-address=192.168.0.39 --pod-network-cidr=192.168.0.0/16
Your Kubernetes master has initialized successfully!
```

Then check similar issues https://github.com/kubernetes/minikube/issues/1546, as currently VirtualBox is not using NAT config but Bridged config, we could simply setup cluster and install flannel (this working one I have switch my node and master)
```
$ kubeadm init --pod-network-cidr=192.168.0.0/16
...
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
 ... 
 
# config the cluster info, if you check the file
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
# or to change permanently append below in ~/.profile 
$ export KUBECONFIG=/etc/kubernetes/admin.conf

$ nano /etc/kubernetes/admin.conf
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQUR$
    server: https://192.168.0.40:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LSO ...
    client-key-data: LS...
...    
    
$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml
tation/kube-flannel.yml
clusterrole "flannel" created
clusterrolebinding "flannel" created
serviceaccount "flannel" created
configmap "kube-flannel-cfg" created
daemonset "kube-flannel-ds" created

$ kubectl cluster-info
Kubernetes master is running at https://192.168.0.40:6443
KubeDNS is running at https://192.168.0.40:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

Now connect to another node and add it to the cluster
```
$ kubeadm join --token b256d0.634136851e9fc44e 192.168.0.40:6443 --discov ery-token-ca-cert-hash sha256:3dfbc5d49cf27805661b37ed68a3a8a67d0b2c390e2b30bd14cc360c5341f113
[preflight] Running pre-flight checks.
        [WARNING FileExisting-crictl]: crictl not found in system path
[preflight] Starting the kubelet service
[discovery] Trying to connect to API Server "192.168.0.40:6443"
[discovery] Created cluster-info discovery client, requesting info from "https://192.168.0.40:6443" [discovery] Requesting info from "https://192.168.0.40:6443" again to validate TLS against the pinned public key
[discovery] Cluster info signature and contents are valid and TLS certificate validates against pinned roots, will use API Server "192.168.0.40:6443"
[discovery] Successfully established connection with API Server "192.168.0.40:6443"

This node has joined the cluster:
* Certificate signing request was sent to master and a response
  was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the master to see this node join the cluster.
```

Switch back to cluster master and verify newly added node
```
$ kubectl get nodes
NAME         STATUS    ROLES     AGE       VERSION
kubemaster   Ready     <none>    32m       v1.9.0
kubeslave1   Ready     master    37m       v1.9.0

$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                 READY     STATUS              RESTARTS   AGE
kube-system   etcd-kubeslave1                      1/1       Running             0          3m
kube-system   kube-apiserver-kubeslave1            1/1       Running             0          3m
kube-system   kube-controller-manager-kubeslave1   1/1       Running             0          4m
kube-system   kube-dns-6f4fd4bdf-7xjzn             0/3       ContainerCreating   0          4m
kube-system   kube-flannel-ds-pzd8h                2/2       Running             0          28s
kube-system   kube-flannel-ds-s8bgm                1/2       CrashLoopBackOff    3          1m
kube-system   kube-proxy-6z5zt                     1/1       Running             0          28s
kube-system   kube-proxy-lbdd9                     1/1       Running             0          4m
kube-system   kube-scheduler-kubeslave1            1/1       Running             0          3m
```

Do not check `nslookup kubemaster` if using kubeadm, it seems even worse if you change '/etc/resolv.conf' !
```
$ nslookup kubemaster
can not find
# similar https://superuser.com/questions/708904/why-does-nslookup-return-error-cant-find-host
```
As hostname kubemaster is our hack way to give a DNS name in /etc/hosts file, it bypass the DNS server if your /etc/nsswitch.conf is not changed(http://bencane.com/2013/10/29/managing-dns-locally-with-etchosts/). To make `nslookup kubemaster` also work,
just append test.com after search in /ect/resolv.conf file, as described by https://www.shellhacks.com/setup-dns-resolution-resolvconf-example/
```
# why reboot will overwrite this change ???
# Generated by NetworkManager
search local test.com
nameserver 192.168.0.1
```

https://www.mirantis.com/blog/how-install-kubernetes-kubeadm/   similar example to deploy a website.    
https://www.profiq.com/kubernetes-cluster-setup-using-virtual-machines/  similar example to deploy cluster with 3 nodes.   


# trouble shoot:

1. Below error happens when you run docker in a VM or AWS environment, you need to restart docker service
  [docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.](https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-at-unix-var-run-docker-sock-is-the-docker-daemon-running/32818/3)
  ```
  sudo service docker stop
  sudo service docker start
  ```

2. docker config
  [image location change](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169)
  CAUTION: on CentOS 7, don't follow this.

  You can change Docker's storage base directory (where container and images go) using the -g option when starting the Docker daemon.

  Ubuntu/Debian: edit your /etc/default/docker file with the -g option: DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -g /mnt"

  Fedora/Centos: edit /etc/sysconfig/docker, and add the -g option in the other_args variable: ex. other_args="-g /var/lib/testdir". If   there's more than one option, make sure you enclose them in " ". After a restart, (service docker restart) Docker should use the new 
  directory.

  Using a symlink is another method to change image storage.

3. can't load image `docker pull python` python is an available image in Dockerhub  
  [question](https://stackoverflow.com/questions/23111631/cannot-download-docker-images-behind-a-proxy)   
  [HTTP/HTTPS proxy doc](https://docs.docker.com/engine/admin/systemd/#runtime-directory-and-storage-driver)   
  
  A quick outline of using systemctl :
  First, create a systemd drop-in directory for the docker service:  
  ```
  sudo mkdir -p /etc/systemd/system/docker.service.d
  ```  
  Now create a file called `/etc/systemd/system/docker.service.d/http-proxy.conf` 
  that adds the HTTP_PROXY environment variable:
  ```
  [Service]
  Environment="HTTP_PROXY=http://proxy.example.com:80/"
  ```
  and/or create a file called `/etc/systemd/system/docker.service.d/https-proxy.conf` that adds the HTTPS_PROXY environment  
  If you have internal Docker registries that you need to contact without proxying you can specify them via the NO_PROXY 
  environment variable:
  ```
  Environment="HTTP_PROXY=http://proxy.example.com:80/"
  Environment="NO_PROXY=localhost,127.0.0.0/8,docker-registry.somecorporation.com"
  ```
  Flush changes:
  `$ sudo systemctl daemon-reload`
  Verify that the configuration has been loaded:
  ```
  $ sudo systemctl show --property Environment docker
  Environment=HTTP_PROXY=http://proxy.example.com:80/
  ```
  Restart Docker:
  `$ sudo systemctl restart docker`

4. [How to mount host directory in docker container?](https://stackoverflow.com/questions/23439126/how-to-mount-host-directory-in-docker-container)   
  to mount the the current directory (source) with /test_container (target) we are going to use:
  ```
  docker run -it --mount src="$(pwd)",target=/test_container,type=bind k3_s3
  ```

5. Dockerfile to create your own image
  ```
  $ FROM python COPY . /src CMD [“python”, “/src/birthday.py”]
  FROM requires either one or three arguments
  ```

6. More issues with k8s deploy is in repo https://github.com/YDD9/docker-python-hello-kubernetes
