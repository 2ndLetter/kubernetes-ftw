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

    1  k get po -A
    2  cd /etc/kubernetes/manifests/
    3  lsw
    4  ls
    5  ls
    6  cat kube-apiserver.yaml 
    7  vim kube-apiserver.yaml 
    8  k get roles
    9  k get roles -a
   10  k get roles -A
   11  k get roles -A --no-headers | wc -l
   12  k describe -n kube-system role kube-proxy 
   13  k describe -n kube-system rolebindings kube-proxy 
   14  kubectl config view
   15  k get pods --as dev-user
   16  kubectl create role developer --verb=list,create,delete --resource=pods
   17  k get role developer 
   18  k get role developer -o yaml
   19  k describe role developer 
   20  k create rolebinding dev-user-binding --role=developer --user=dev-user
   21  k get roles
   22  k get rolebindings
   23  k get role developer -o yaml
   24  k get rolebinding dev-user-binding -o yaml
   25  k describe role developer 
   26  k get role developer -o yaml
   27  k get role developer -o yaml
   28  k edit role developer 
   29  k get pods -A
   30  k describe -n blue po dark-blue-app --as dev-user
   31  k edit role developer 
   32  k get -n blue role developer 
   33  k get -n blue role developer -o yaml
   34  k edit -n blue role developer 
   35  k describe -n blue po dark-blue-app --as dev-user
   36  k edit -n blue role developer 
   37  kubectl api-resources
   38  kubectl api-resources | grep -i deployment
   39  k edit -n blue role developer 
   40  k create deployment nginx --image=nginx --as dev-user -n blue
   41  history