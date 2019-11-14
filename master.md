# Setting up a master with kubeadm

## Pre-reqs

Node must already be setup with kubeadm - see node.md for instructions.

## Initialize cluster

_On the master node_

See https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/ for details.

We will be using flannel as our CNI during our cluster init.

For flannel run:

`kubeadm init --pod-network-cidr=10.244.0.0/16`

The command outputs a success message if it runs through correctly. If everything works, then the control plane is now setup on the node.

Config file for the node is at `/etc/kubernetes/admin.conf`.

Setting `export KUBECONFIG=/etc/kubernetes/admin.conf` allows you to run kubectl from a ssh session *on* the node.

## Install flannel

_On the master node_

Run `export KUBECONFIG=/etc/kubernetes/admin.conf` in the console to set kubectl config.
At this point `kubectl get nodes` should show our master as *Not Ready*. Need to setup CNI.

Set net to bridge IPv4 traffic to iptable chains (because [reasons](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#network-plugin-requirements)).

`sysctl net.bridge.bridge-nf-call-iptables=1`

Install flannel addon

`kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/62e44c867a2846fefb68bd5f178daf4da3095ccb/Documentation/kube-flannel.yml`

## Untaint master (optional?)

_On the master node_

By default, the master node is tainted to prevent pod scheduling. This is to ensure that only the control plane runs on the master. If you want to run pods on your master you will need to untaint the master.

Untaint nodes
`kubectl taint nodes --all node-role.kubernetes.io/master-`
