# API Groups:
## Documentation:
- https://kubernetes.io/docs/reference/using-api/

## Notes/Commands:
- APIs:
  ```
    - /metrics - Monitor cluster health
    - /healthz - Monitor cluster health
    - /version - Version of cluster
    - /logs - 3rd party logging application integration
    - /api                   # Core API Group
      - /v1:
        - namespaces
        - events
        - bindings
        - configmaps
        - pods
        - endpoints
        - PV
        - secrets
        - rc
        - nodes
        - PVC
        - services
    - /apis                  # Named API Group
      - /apps                  # API Groups
        - /v1
          - /deployments         # Resources
            - list                 # Verbs (actions)
            - get                  # Verbs (actions)
            - create               # Verbs (actions)
            - delete               # Verbs (actions)
            - update               # Verbs (actions)
            - watch                # Verbs (actions)
          - /replicasets         # Resources
          - /statefulsets        # Resources
      - /extentions            # API Groups
      - /networking.k8s.io     # API Groups
        - /v1
          - /networkpolicies
      - /storage.k8s.io        # API Groups
      - /authentication.k8s.io # API Groups
      - /certificates.k8s.io   # API Groups
        - /v1
          - /certificatesigningrequests
  ```
- `curl http://localhost:6443 -k`
  ```json
  {
    "paths": [
      "/api",
      "/api/v1",
      "/apis",
      "/apis/",
      ...omitted...
  ```
  - You have to pass in your certificate files in order to use these curl commands, or (do the below)
- `kubectl proxy`
  ```
  Starting to serve on 127.0.0.1:8001
  ```
  - This launches a proxy service on local port 8001
  - Uses ".kube/config" credentials to access the cluster
  - So you don't have to include certificates in each curl command
- kube proxy != kubectl proxy
  - kube proxy: Used to enable access to K8s cluster
  - kubectl proxy: http proxy service created by kube control utility to access the kube api server
