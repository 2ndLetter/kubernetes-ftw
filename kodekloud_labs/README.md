# [KodeKloud Labs](https://kodekloud.com/courses/labs-certified-kubernetes-administrator-with-practice-tests/)
---
# ==Core Concepts==
## PODs:
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
- `kuberctl delete -f nginx.yaml` # Delete Objects
### Imperative workflow:
1. `kubectl create -f nginx.yaml`
2. `kubectl edit deployment nginx`
3. `kubectl replace -f nginx.yaml` # Add the "--force" option to immediately remove/recreate objects
### Declarative example:
- `kubectl apply -f nginx.yaml`
- `kubectl apply -f /path /to/config-files` # Creates using multiple manifest files within a directory
### Imperative Commands to view/generate manifest files:
- `kubectl <command> <command_options> --dry-run=client -o yaml` # View
- `kubectl <command> <command_options> --dry-run=client -o yaml > path/to/manifest/file.yaml` # Generate
- `kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml`
- `kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`
- `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml`
  - Uses Pod's labels as selectors
  - Creates ClusterIP by default
- `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml`
  - Assumes selectors is app=redis
  - You cannot pass in selectors as an option, so generate the file and update it
  - Creates ClusterIP by default
- `kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml`
  - You cannot specify the node port, so generate the file and update it
  - Creates NodePort
- `kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml`
  - This will NOT use the pods labels as selectors
  - Creates NodePort
### Service Creation Summary:
- `kubectl create service ...` # Can't accept a selector
- `kubectl expose pod ...` # Can't accept a node port
- Recommend using `kubectl expose ...`
  - if you need to specify a node port, generate a manifest file and manually input the node port
### Lab Commands used:
- `kubectl run redis --image=redis:alpine --labels="tier=db"`
- `kubectl expose pod redis --port=6379 --name=redis-service`
- `kubectl create namespace dev-ns`
- `kubectl create deployment redis-deploy -n dev-ns --image=redis --replicas=2`
- `kubectl run httpd --image=httpd:alpine --expose --port=80`
---

# ==Scheduling==
## Manual Scheuling:
- `kubectl apply -f nginx.yaml`
- `kubectl replace --force -f nginx.yaml`
## Labels and Selectors:
- `kubectl ...`
## Taints and Tolerations:
- `kubectl ...`
## Node Affinity:
- `kubectl ...`
## Resource Limits:
- `kubectl ...`
## DaemonSets:
- `kubectl ...`
## Static PODs:
- `kubectl ...`
## Multiple Schedulers:
- `kubectl ...`
---

# ==Logging & Monitoring==
## Monitor Cluster Components:
- `kubectl ...`
## Managing Application Logs:
- `kubectl ...`
---

# ==Application Lifecycle Management==
## Rolling Updates and Rollbacks:
- `kubectl ...`
## Commands and Arguments:
- `kubectl ...`
## Env Variables:
- `kubectl ...`
## Secrets:
- `kubectl ...`
## Multi Container PODs:
- `kubectl ...`
## Init Containers:
- `kubectl ...`
---

# ==Cluster Maintenance==
## OS Upgrades:
- `kubectl ...`
## Cluster Upgrade Process:
- `kubectl ...`
## Backup and Restore Methods:
- `kubectl ...`
## Backup and Restore Methods 2:
- `kubectl ...`
---

# ==Security==
---

# ==Networking==
---

# ==Install==
---

# ==Troubleshooting==
---

# ==Other Topics==
---

# ==Lighting Labs==
---

# ==Mock Exams==
---
