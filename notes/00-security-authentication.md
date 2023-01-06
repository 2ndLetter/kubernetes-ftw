# Section Title:
## Documentation:
- https://kubernetes.io/docs/home/

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
- Authentication Mechanisms: (kube-apiserver)
  - Static Password File
    - user-details.csv:
    ```
    password123,user1,u0001
    password123,user2,u0002
    password123,user3,u0003
    ```
    kube-apiserver.service:
    ```
    ....
    --basic-auth-file=user-details.csv
    ...
    ```
  - Static Token File
  - Certificates
  - Identity Services

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
