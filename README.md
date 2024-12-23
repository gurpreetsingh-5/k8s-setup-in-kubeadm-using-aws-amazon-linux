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
