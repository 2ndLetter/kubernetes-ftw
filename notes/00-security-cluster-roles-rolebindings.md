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

    1  kubectl get clusterroles
    2  kubectl get clusterroles --no-headers
    3  kubectl get clusterroles --no-headers | wc -l
    4* kubectl get clusterrolebindings --no-hea
    5  kubectl get clusterroles -o wide
    6  kubectl get clusterrole cluster-admin
    7  k describe clusterrole cluster-admin
    8  k describe clusterrolebinding cluster-admin
    9  ls
   10  kubectl config view
   11  vim clusterrole.yaml
   12  ls
   13* kubectl create clusterrole cluster-role-storage --verb=get,list,watch,create,delete --resource=nodes --dry-run=client -o yaml
   14  kubectl create clusterrole cluster-role-nodes --verb=get,list,watch,create,delete --resource=nodes --dry-run=client -o yaml > cluster-role-nodes.yaml
   15  kubectl create clusterrolebinding cluster-role-nodes-binding --clusterrole=cluster-role-nodes --user=michelle --dry-run=client -o yaml
   16  kubectl create clusterrolebinding cluster-role-nodes-binding --clusterrole=cluster-role-nodes --user=michelle --dry-run=client -o yaml
   17  kubectl create clusterrolebinding cluster-role-nodes-binding --clusterrole=cluster-role-nodes --user=michelle --dry-run=client -o yaml > cluster-role-nodes-binding.yaml
   18  ls
   19  kubectl apply -f cluster-role-nodes.yaml 
   20  kubectl apply -f cluster-role-nodes-binding.yaml
   21  kubectl api-resources
   22  kubectl api-resources --namespaces=true
   23  kubectl api-resources -h
   24  kubectl api-resources --namespaced=true
   25  kubectl api-resources --namespaced=false
   26  kubectl api-resources --namespaced=false
   27  echo "PersistentVolume,StorageClass" > api-resources
   28  cat api-resources 
   29  kubetl create clusterrole storage-admin --resource= -h
   30  cat api-resources 
   31  kubectl create clusterrole storage-admin --resource=PersistentVolume,StorageClass --verb= --help
   32  kubectl create clusterrole storage-admin --resource=PersistentVolume,StorageClass --verb=get,watch,list,create,delete --dry-run=client
   33  kubectl create clusterrole storage-admin --resource=PersistentVolume,StorageClass --verb=get,watch,list,create,delete --dry-run=client -o yaml
   34  kubectl create clusterrole storage-admin --resource=PersistentVolume,StorageClass --verb=get,watch,list,create,delete --dry-run=client -o yaml > storage-admin.yaml
   35  vim storage-admin.yaml 
   36  kubectl create clusterrolebinding michelle-storage-admin --clusterrole=storage-admin --user=michelle --dry-run=client -o yaml
   37  kubectl create clusterrolebinding michelle-storage-admin --clusterrole=storage-admin --user=michelle --dry-run=client -o yaml > michelle-storage-admin.yaml
   38  ls
   39  ls -l
   40  kubectl apply -f storage-admin.yaml 
   41  kubectl apply -f michelle-storage-admin.yaml 
   42  ls
   43  cat api-resources 
   44  ls
   45  cat cluster-role-nodes.yaml 
   46  ls
   47  cat cluster-role-nodes-binding.yaml 
   48  cat storage-admin.yaml
   49  cat michelle-storage-admin.yaml 
   50  history

    1  k get clusterrole
    2  k get clusterroles
    3  k get clusterrole
    4  k get clusterroles --no-headers | wc -l
    5  k get clusterrolebindings.rbac.authorization.k8s.io --no-headers | wc -l
    6  k get clusterrole cluster-admin 
    7  k get clusterrole cluster-admin -o yaml
    8  k get clusterrole -o wide
    9  k describe clusterrole cluster-admin 
   10  k describe clusterrole cluster-admin -o wide
   11  k describe clusterrole cluster-admin 
   12  k get clusterrole cluster-admin -o yaml
   13  k get clusterrolebindings.rbac.authorization.k8s.io 
   14  k get clusterrolebindings.rbac.authorization.k8s.io | grep cluster-admin
   15  k describe clusterrolebindings.rbac.authorization.k8s.io cluster-admin
   16  k describe clusterrole cluster-admin 
   17  k create clusterrole michelle-role --verb=create,list,get,delete --resource=nodes
   18  k create clusterrolebinding michelle-role-binding --user=michelle --clusterrole=michelle-role
   19  k get api-resource
   20  k get api-resources
   21  kubectl api-resources
   22  kubectl api-resources | grep persistent
   23  k create clusterrole storage-admin --verb=create,list,get,delete --resource=persistentvolumes,storageclasses
   24  k create clusterrolebinding michelle-storage-admin --user=michelle --clusterrole=storage-admin