# Joining a worker node to a master node

## Pre-reqs

Both worker and master must already be setup correctly. See node.md and master.md for details.
You will also need the IP or hostname of the master node.

## Get token

Get token from master node
`kubeadm token list`

## Get discovery token ca cert hash

Run this command
```
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
openssl dgst -sha256 -hex | sed 's/^.* //'
```

## Join worker

*On the worker node*
Run this command
`kubeadm join --token <token> <master-ip or hostname>:6443 --discovery-token-ca-cert-hash sha256:<discovery token ca cert hash>`
