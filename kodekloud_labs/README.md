# [KodeKloud Labs](https://kodekloud.com/courses/labs-certified-kubernetes-administrator-with-practice-tests/)

# Core Concepts:
## PODs: (video 27)
- Commands used:
  - `kubectl get pods -o wide`
  - `kubectl run <pod_name> --image=<image_name>`
  - `kubectl describe pod <pod_name>`
  - `kubectl delete pod <pod_name>`
  - `kubectl run redis --image=redis --dry-run=client -o yaml > redis.yml`
    - Manually update file as required (via vim)
    - `kubectl create -f redis.yml`
## ReplicaSets:
- Commands used:
  - `kubectl create -f replicaset-definition.yml`
  - `kubectl get replicaset`
  - `kubectl delete replicaset myapp-replicaset`
  - `kubectl replace -f replicaset-definition.yml`
  - `kubectl scale -replicas=6`
## Deployments:
- tbd
## Namespaces:
- tbd
## Services:
- tbd
## Imperative Commands:
- tbd




# Scheduling:

# Logging & Monitoring:

# Application Lifecycle Management:

# Cluster Maintenance:

# Security:

# Networking:

# Install:

# Troubleshooting:

# Other Topics:

# Lighting Labs:

# Mock Exams:

