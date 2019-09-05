# Setting up a node with kubeadm

## Install nano

Because text-editor.

`yum install nano`

## Turn off swap

Turn it off

`swapoff -a`

Verify it turned off (show return nothing)

`swapon --show`

Delete any lines in `/etc/fstab` that reference swap.

`nano /etc/fstab`

## Disable firewalld

`systemctl stop firewalld`

`systemctl disable firewalld`

Verify that firewalld is disabled

`systemctl status firewalld`

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


## Install and run kubeadm

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

## Setup samba (optional)

Install samba service

`sudo apt install samba`

Create user for a share

`smbpasswd -a <username>`

Update `/etc/samba/smb.conf` file with share details

```
[<share folder name>]
  path = <share path on node>
  available = yes
  valid uesrs = <username>
  read only = no
  browsable = yes
  public = yes
  writable = yes
```

Restart samba service

`sudo service smbd restart`

You can now connect to the samba share via IP address and share name. Use the credentials created above for access.

## Installing Java - CentOS (optional)

Update

`yum update`

Install java

```
sudo yum install java-1.8.0-openjdk.x86_64
sudo cp /etc/profile /etc/profile_backup
echo 'export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk' | sudo tee -a /etc/profile 
echo 'export JRE_HOME=/usr/lib/jvm/jre' | sudo tee -a /etc/profile source /etc/profile
```

## Installing Helm

Install helm

`curl -L https://git.io/get_helm.sh | bash`

Helm CLI uses kube config file at `~/.kube/config`

## Installing Java - Raspian - _incomplete_ (optional)

Need to install binary from here: https://www.oracle.com/technetwork/java/javase/downloads/jdk12-downloads-5295953.html
and get it onto the server somehow. I used a samba share... ugh.

Unzip tar

`tar -xvf jdk-12.0.2_linux-x64_bin.tar.gz`

Create a directory for the jvm at `/usr/lib/jvm`

`sudo mkdir -p /usr/lib/jvm`

Move java directory there

`sudo mv ./jdk-12.0.2 /usr/lib/jvm`

Then run

```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk-12.0.2/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk-12.0.2/bin/javac" 1
sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk-12.0.2/bin/javaws" 1
```

## Fin

This node is now setup with kubelet and kube-proxy. It is awaiting additional commands from kubeadm (either 'init' to create a cluster or 'join' to join an existing cluster).
