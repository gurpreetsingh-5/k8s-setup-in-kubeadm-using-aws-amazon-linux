# Kubernetes :- Setup k8s Production grade container orchestration Engine
## Kubernetes multinode setup
### we have 3 machines; 1 master and 2 worker nodes
## Pre-requisite
+   Amazon linux 2 Ami OS 
+   master t2.medium and worker t2.micro
## Installing docker in all the nodes
    yum  install  docker  -y
## Now install kubeadm and configure yum
you can browse this link [kubernetes repo](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)  <br/>
## OR 
## yum can be configured by running this command 
    cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://pkgs.k8s.io/core:/stable:/v1.30/rpm/
    enabled=1
    gpgcheck=1
    gpgkey=https://pkgs.k8s.io/core:/stable:/v1.30/rpm/repodata/repomd.xml.key
    exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
    EOF
## now run 
    yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes --disableplugin=priorities

## Start service of docker & kubelet in all the nodes 
     systemctl enable --now  docker kubelet

## for kernel configuration and ip tables
```
modprobe br_netfilter <br/>
```
```
echo '1' > /proc/sys/net/ipv4/ip_forward 
```
```
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
```

 # Do this only on Kubernetes Master 
 
 We are here using Calico Networking, so we need to pass some parameter 
 you can start [Kubernetes_networking](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) from this  <br/>

 ```
kubeadm  init --pod-network-cidr=192.168.0.0/16
```
# OR
## In case of cloud services like aws, azure if want to bind public with certificate of kubernetes 
```
kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=0.0.0.0   --apiserver-cert-extra-sans=publicip,privateip,serviceip
```

 ## Use the output of above command and paste it to all the worker nodes
