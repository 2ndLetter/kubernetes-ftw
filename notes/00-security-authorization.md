# Authorization:
## Documentation:
- https://kubernetes.io/docs/home/

## Notes/Commands:
- Authorization Mechanisms:
  - Node:
  - ABAC:
  - RBAC:
  - Webhook:

- Node Authorizer:
  - Part of the SYSTEM:NODES group
  - sysem:node:node01, using the kubelet, is authorized by the Node Authorizer
- ABAC:
  - tbd
- RBAC:
  - tbd
- Webhook:
  - tbd












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
