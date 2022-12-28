# [KodeKloud Labs](https://kodekloud.com/courses/labs-certified-kubernetes-administrator-with-practice-tests/)

# Core Concepts:

## PODs: (video 27)
- `kubectl get pods -o wide`
- `kubectl run <pod_name> --image=<image_name>`
- `kubectl describe pod <pod_name>`
- `kubectl delete pod <pod_name>`
- `kubectl run redis --image=redis --dry-run=client -o yaml > redis.yml`
  - Manually update file as required (via vim)
  - `kubectl create -f redis.yml`

## ReplicaSets:
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
### Imperative examples:
- `kubectl run --image=nginx nginx` # Create Objects
- `kubectl create deployment --image=nginx nginx` # Create Objects
- `kubectl create -f nginx.yaml` # Create Objects
- `kubectl expose deployment nginx --port 80` # Create Objects
- `kubectl scale deployment nginx --replicas=5` # Update Objects
- `kubectl set image deployment nginx nginx=nginx:1.18` # Update Objects
- `kubectl edit deployment nginx` # Update Objects
- `kubectl replace -f nginx.yaml` # Update Objects
- `kuberctl delete -f nginx.yaml`
### Imperative workflow:
1. `kubectl create -f nginx.yaml`
2. `kubectl edit deployment nginx`
3. `kubectl replace -f nginx.yaml` # Add the "--force" option to immediately remove/recreate objects
### Delcarative example:
- `kubectl apply -f nginx.yaml`
- `kubectl apply -f /path /to/config-files` # Creates using multiple manifest files within a directory

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

