# Setting up a node with kubeadm

## Turn off swap

`swapoff -a`

Delete any lines in `/etc/fstab` that reference swap.
`nano /etc/fstab`

## Disable firewalld

`systemctl stop firewalld`
`systemctl disable firewalld`

Verify that firewalld is disabled
`systemctl status firewalld`

## Install nano

Because text-editor.
`yum install nano`

## Install Docker

Update
`yum check-update`

Install Docker (for CentOs...)
`curl -fsSL https://get.docker.com/ | sh`

Start Docker
`systemctl start docker`

Verify Docker started
`systemctl status docker`

Enable Docker at reboot
`systemctl enable docker`


## Run kubeadm
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable --now kubelet
```