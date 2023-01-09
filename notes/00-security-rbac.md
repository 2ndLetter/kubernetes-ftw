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
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep authorization`
  - Another method: `ps -aux | grep authorization`
- `kubectl get roles`
- `kubectl get roles -A --no-headers | wc -l`
- `kubectl describe -n kube-system role kube-proxy`
- `kubectl get -n kube-system rolebindings`
- `kubectl describe -n kube-system rolebindings kube-proxy`
- `kubectl config view`
- `k get pods --as dev-user`
- `kubectl auth can-i get pods --as dev-user`
- `kubectl create role developer --verb=list,create,delete --resource=pods --dry-run=client -o yaml > developer-role.yaml`
- developer-role.yaml:
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    creationTimestamp: null
    name: developer
  rules:
  - apiGroups:
    - ""
    resources:
    - pods
    verbs:
    - list
    - create
    - delete
  ```
- `kubectl describe role developer`
- `kubectl create rolebinding dev-user-binding --user=dev-user --role=developer --dry-run=client -o yaml > developer-role.yaml`
- dev-user-binding.yaml:
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    creationTimestamp: null
    name: dev-user-binding
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: developer
  subjects:
  - apiGroup: rbac.authorization.k8s.io
    kind: User
    name: dev-user
  ```
  `kubectl describe rolebinding dev-user-binding`
- `kubectl apply -f developer-role.yaml`
- `kubectl apply -f dev-user-binding.yaml`
- `kubectl get ...`