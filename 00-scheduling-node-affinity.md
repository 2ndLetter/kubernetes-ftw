## Node Affinity - PODs: [documentation](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: data-processor
spec:
  containers:
  - name: data-processor
    image: data-processor
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In | NotIn | Exists
            values:
            - Large
            - Medium
```