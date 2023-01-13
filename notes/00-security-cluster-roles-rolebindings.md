# Cluster Roles and Role Bindings:
## Documentation:
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/

## Notes/Commands:
- Roles/RoleBindings are namespace scoped
- ClusterRoles/ClusterRoleBindings are cluster scoped
  - You *can* create a ClusterRole for a non-cluster wide resource
  - It will have access to that resource across all namespaces
- `kubectl api-resources --namespaced=true`  # View namespace scoped resources
- `kubectl api-resources --namespaced=false` # View Cluster scoped resources

## Usage:
- Imperative Way:
  - https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-clusterrole
- Delcarative Way:
  - https://kubernetes.io/docs/reference/access-authn-authz/rbac/#clusterrole-example




## KodeKloud Lab:
- ``
- `kubectl api-resources --namespaced=false`

api-resources:
PersistentVolume,StorageClass

cluster-role-nodes.yaml:
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  creationTimestamp: null
  name: cluster-role-nodes
rules:
- apiGroups:
  - ""
  resources:
  - nodes
  verbs:
  - get
  - list
  - watch
  - create
  - delete

cluster-rolenodes-binding.yaml:
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  creationTimestamp: null
  name: cluster-role-nodes-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-role-nodes
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: michelle

storage-admin.yaml:
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  creationTimestamp: null
  name: storage-admin
rules:
- apiGroups:
  - ""
  resources:
  - persistentvolumes
  verbs:
  - get
  - watch
  - list
  - create
  - delete
- apiGroups:
  - storage.k8s.io
  resources:
  - storageclasses
  verbs:
  - get
  - watch
  - list
  - create
  - delete


michell-storage-admin.yaml:
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  creationTimestamp: null
  name: michelle-storage-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: storage-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: michelle

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