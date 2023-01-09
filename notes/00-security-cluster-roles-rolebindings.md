# Cluster Roles and Role Bindings:
## Documentation:
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/

## Notes/Commands:
- Roles/RoleBindings are namespace scoped
- ClusterRoles/ClusterRoleBindings are cluster scoped
- `kubectl api-resources --namespaces=true`  # View namespace scoped resources
- `kubectl api-resources --namespaces=false` # View Cluster scoped resources

















## Usage:
- Imperative Way:
  - https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-clusterrole
- Delcarative Way:
  - https://kubernetes.io/docs/reference/access-authn-authz/rbac/#clusterrole-example

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
