## On tiller not found error create following yaml-file

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
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```


## Get old tiller replicaset name
```
kubectl -n kube-system get replicaset
```

## Delete old replicaset 
```
kubectl -n kube-system delete replicaset tiller-deploy-xxxxxxx
```

## Init helm
```
helm init
```
