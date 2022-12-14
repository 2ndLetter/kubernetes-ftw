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
- `kubectl --as dev-user get pod dark-blue-app -n blue`
- `k get roles -n blue`
- `k get rolebindings -n blue`
- `k describe -n blue role developer`
- Fix the Resource name
- `k get -n blue role developer -o yaml > dark-blue-app-role.yaml`
- `k replace --force -f dark-blue-app-role.yaml`
- Would have been faster if I edited the object in place (imperative):
  - `k edit role developer -n blue`
- `k --as dev-user create deployment nginx --image=nginx n blue`
  - This is just a test, to receive the error
- `k edit role developer -n blue`
  - Add new rule, don't forget the apiGroups
  - Save time and copy the existing rule, and paste at the end
  - Save and exit
  ```yaml
  ...OMITTED...
  rules:
  - apiGroups:
    - apps
    resources:
    - deployments
    verbs:
    - create
  ```
  - `k describe role developer -n blue`
  - `kubectl auth can-i create deployment --as dev-user -n blue`
  - `k --as dev-user create deployment nginx --image=nginx -n blue`
  - history output:
    - k get po -A
    - cd /etc/kubernetes/manifests/
    - Another way: ps -aux | grep authorization
    - lsw
    - ls
    - ls
    - cat kube-apiserver.yaml 
    - vim kube-apiserver.yaml 
    - k get roles
    - k get roles -a
    - k get roles -A
    - k get roles -A --no-headers | wc -l
    - k describe -n kube-system role kube-proxy 
    - k describe -n kube-system rolebindings kube-proxy 
    - kubectl config view
    - k get pods --as dev-user
    - kubectl create role developer --verb=list,create,delete --resource=pods
    - k get role developer 
    - k get role developer -o yaml
    - k describe role developer 
    - k create rolebinding dev-user-binding --role=developer --user=dev-user
    - k get roles
    - k get rolebindings
    - k get role developer -o yaml
    - k get rolebinding dev-user-binding -o yaml
    - k describe role developer 
    - k get role developer -o yaml
    - k get role developer -o yaml
    - k edit role developer 
    - k get pods -A
    - k describe -n blue po dark-blue-app --as dev-user
    - k edit role developer 
    - k get -n blue role developer 
    - k get -n blue role developer -o yaml
    - k edit -n blue role developer 
    - k describe -n blue po dark-blue-app --as dev-user
    - k edit -n blue role developer 
    - kubectl api-resources
    - kubectl api-resources | grep -i deployment
    - k edit -n blue role developer 
    - k create deployment nginx --image=nginx --as dev-user -n blue
    - history