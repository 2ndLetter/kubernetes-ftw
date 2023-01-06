# TLS:
## Documentation:
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/

## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
## Commands:
- `kubectl get ...`
- `kubectl describe ...`

## Notes:

## Server Certificates:
- KUBE-API SERVER:
  - apiserver.crt (certificate)
  - apiserver.key (priv key)
- ETCD SERVER:
  - etcdserver.crt (certificate)
  - etcdserver.key (priv key)
- KUBELET SERVER:
  - kubelet.crt (certificate)
  - kubelet.key (priv key)
## Client Certificates:
- ADMIN: (kubectl REST API)
  - admin.crt (certificate)
  - admin.key (priv key)
- 



## KodeKloud Lab:
- `kubectl ...`
- `kubectl ... --dry-run=client -o yaml > template.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
- `kubectl apply -f template.yaml`
