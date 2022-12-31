## Limit Range Example (Memory): [documentation](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

## Limit Range Example (CPU): [documentation](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

## Resource Limits:
- `kubectl get pod webapp -o yaml > my-new-pod.yaml` # Generate a manifest file from a running resource
  - Edit the manifest file via VIM
  - Delete the resource
  - Re-create using the newly generated manifest file
- `kubectl edit deployment my-deployment` # Edit a pod that is part of a deployment
- `kubectl get pod elephant -o yaml > elephant.yaml`
  - Edit the manifest file via VIM
  - `kubectl replace --force -f elephant.yaml`
  - `kubect delete po elephant`
- OOMKilled means the pod ran out of Memory