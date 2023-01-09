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
