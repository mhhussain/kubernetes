## Pre-reqs

k8s cluster with cluster-admin role.

## Install kubernetes dashboard

Run

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta1/aio/deploy/recommended.yaml`

## Installing Helm

Create a service account for tiller using the following yaml

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
subjects:
- kind: ServiceAccount
  name: tiller
  namespace: kube-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
```
Note, that tiller does require cluster-admin.

Run helm init

`helm init --service-account tiller --history-max 200`

## Installing Ambassador

Install Ambassador and create rbac:

`kubectl apply -f https://getambassador.io/yaml/ambassador/ambassador-rbac.yaml`

Create Ambassador service

```
apiVersion: v1
kind: Service
metadata:
  name: ambassador
spec:
  type: NodePort
  externalTrafficPolicy: Local
  ports:
  - port: 80
    targetPort: 8080
    nodePort: 31000
  selector:
    service: ambassador
```

## Installing Jenkins

_for continuous building_
