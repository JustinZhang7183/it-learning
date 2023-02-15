# Environment
### Container Runtimes
##### Configure prerequisites
- Forwarding IPv4 and letting iptables see bridged traffic
- set config using systemd cgroupdriver
```
# for docker
vim /etc/docker/daemon.json
{
    "exec-opts": ["native.cgroupdriver=systemd"]
}
# then restart
```
##### Cgroup drivers
- Both kubeadm and container runtime need to config
##### Container Runtimes
- containerd
- CRI-O
- Docker Engine (cri-dockerd, dockershim had been removed)
- Mirantis Container Runtime (cri-dockerd)
### Learning Environment
##### Tools
- kubectl
- kind
- minikube
### Production environment
##### Requirements
- Availability, Scale, Security and access management
##### Tools
- kubeadm
- kops
- kubespray
##### Set up control plane
- Manage certificates (automatical or custom)
- Configure load balancer for apiserver
- Separate and backup etcd service
- Create multiple control plane systems
- Span multiple zones
- Manage on-going features (maintain cluster's health and security, upgrading, administering)
##### Set up worker nodes
- Configure nodes (consider appropriate memory, cpu, disk speed and storage capacity available)
- Validate nodes (meet the requirements to join a k8s cluster)
- Add nodes to the cluster
- Scale nodes
- Autoscale nodes (cloud or on-premises with script)
- Set up node health checks
##### User management (Authentication & Authorization)
- Set the authorization mode
- Create user certificates and role bindings (RBAC)
- Create policies that combine attributes (ABAC)
- Consider Admission Controllers (add custom authorization types by it)
##### Set limits on workload resources
- Set namespace limits.
- Prepare for DNS demand: DNS service must scale up as well as workloads
- Create additional service accounts (a pod takes on the default service account from its namespace)
### Bootstrapping clusters with kubeadm
- install kubelet kubeadm kubectl
- need to install cri-docker to communicate with docker-engine
- kubeadm config images pull: pull the images which are necessary when init
```
if your network is limited, set the http_proxy is a good choice.
```
- kubeadm init --config kubeadm.config: create a control plane node in accordance with config file
# English
### Phrases
- take on: **Taking on** a production-quality cluster means ...
- set out: when you **set out** to authorize your regular users, you will ..
### Grammar
- Causative verbs - have, let, make: either adding them manually or **having** them register themselves to the cluster's apiserver.
# Abbreviation
- etcd: 
- LDAP: Lightweight Directory Access Protocol
- RBAC: Role-based access control
- ABAC: Attribute-based access control
- CA: Certificate Authority
- CRI: Container Runtime Interface
- CNI: Container Network Interface