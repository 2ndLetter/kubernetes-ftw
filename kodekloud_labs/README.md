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
  - `kubectl explain replicaset`
  - `kubectl describe ReplicaSets` | `kubect describe rs`
  - `kubectl apply -f replicaset-definition.yml`
  - `kubectl get replicaset`
  - `kubectl delete rs replicaset-1 replicaset2`
  - `kubectl get rs new-replica-set -o yaml > new-replica-set.yml`
    - `kubectl replace -f new-replica-set.yml`
    - `kubectl delete pod pod1 pod2 pod3`
  - `kubectl edit rs new-replica-set`
    - Scale pods up or down, then save
  - `kubectl scale -replicas=2 rs new-replia-set`
## Deployments:
- `kubectl get all`
- `kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yml`
- `kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yml`
## Namespaces:
- `kubectl get ns`
- `kubectl create namespace <namespace_name>`
- `kubectl get pod --all-namespaces`
- `kubectl get pod -A`
## Services:
- `kubectl get service`
- `kubectl get svc`
- `kubectl describe svc <service_name>`
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

