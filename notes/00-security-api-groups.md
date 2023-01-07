# API Groups:
## Documentation:
- https://kubernetes.io/docs/reference/using-api/

## Notes/Commands:
- APIs:
```
  - /metrics - Monitor cluster health
  - /healthz - Monitor cluster health
  - /version - Version of cluster
  - /logs - 3rd party logging application integration
  - /api - core functionality:
    - /v1:
      - namespaces
      - events
      - bindings
      - configmaps
      - pods
      - endpoints
      - PV
      - secrets
      - rc
      - nodes
      - PVC
      - services
  - /apis - named:
    - /apps                  # API Groups
      - /v1
        - /deployments         # Resources
          - list                 # Verbs
          - get                  # Verbs
          - create               # Verbs
          - delete               # Verbs
          - update               # Verbs
          - watch                # Verbs
        - /replicasets         # Resources
        - /statefulsets        # Resources
    - /extentions            # API Groups
    - /networking.k8s.io     # API Groups
      - /v1
        - /networkpolicies
    - /storage.k8s.io        # API Groups
    - /authentication.k8s.io # API Groups
    - /certificates.k8s.io   # API Groups
      - /v1
        - /certificatesigningrequests
```


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
