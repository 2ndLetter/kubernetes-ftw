# Role Based Access Control:
## Documentation:
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/

## Notes/Commands:
- `kubectl get roles`
  ```
  NAME      AGE
  developer 4s
  ```
- `kubectl get rolebindings`
- `kubectl describe role developer`
- `kubectl describe rolebindings`
- `kubectl auth can-i create deployments`
  ```
  yes
  ```
- `kubectl auth can-i delete notes`
  ```
  no
  ```
- `kubectl auth can-i create deployments --as dev-user --namespace test` # To test as a different user

## Usage:
- Imperative Way:
  - Link: https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
- Delcarative Way:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""] # <------ Core Group can be blank, any other group you specify
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
  resourceNames: ["blue", "orange"] #<------- Add permissions to specific resources
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding #<--------- Links User to Role
metadata:
  name: devuser-devloper-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
- kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
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
