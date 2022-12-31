



# ==Logging & Monitoring==
## Monitor Cluster Components:
- `git clone https://github.com/kubernetes-incubator/metrics-server.git`
- kubectl apply -f deploy/1.8+/
- `kubectl top pod`
- `kubectl top node`
## Managing Application Logs:
- `docker logs -f ecf`
- `kubectl top node`
- `kubectl logs -f webapp-1`
- `kubectl logs -f webapp-2 simple-webapp`
- If multiple containers within a Pod, you must provide the Pod and Container name
---

# ==Application Lifecycle Management==
## Rolling Updates and Rollbacks:
- `kubectl rollout status deployment myapp-deployment`
- `kubectl rollout history deployment myapp-deployment`
- `kubectl set image deployment myapp-deployment ngninx=nginx:1.9.1`
- `kubectl rollout undo deployment myapp-deployment`
- `kubectl set image deployment frontend simple-webapp=kodecloud/webapp-color:v2`
## Commands and Arguments:
- `docker run ubuntu sleep 5`
- `docker ps`
- `docker ps -a`
- DOCKERFILE ubuntu-sleeper:
  ```yaml
  FROM Ubuntu
  ENTRYPOINT ["sleep"] # ENTRYPOINT
  CMD ["5"]            # CMD
  ```
  - `docker run ubuntu-sleeper 10`
- pod-definition.yml:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-pod
  spec:
    containers:
      - name: ubuntu-sleeper
        image: ubuntu-sleeper
        command: ["sleep2.0"] # OVERRIDES ENTRYPOINT
        args: ["10"]          # OVERRIDES CMD
  ```
- ubuntu-sleeper-2.yaml:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: ubuntu-sleeper-2
  spec:
    containers:
      - name: ubuntu
        image: ubuntu
        ###
        command: ["sleep", "5000"]
        OR
        command: ["sleep"]
        args: ["5000"]
        OR
        command:
          - "sleep"
          - "5000"
  ```
- `kubectl run webapp-green --image kodekloud/webapp-color --dry-run=client -o yaml --command -- --color=green > ubuntu-sleeper-2.yaml`
- `kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>` # Start default command with custom args
## Env Variables:
- Imperative way:
  - `kubectl create config map <config-name> --from-literal=<key>=<value>`
  - `kubectl create config map app-config --from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod` 
  - `kubectl create configmap app-config --from-file=<path-to-file>`
- Declaritive way:
  - config-map.yaml:
  ```yaml
  apiVersion: v1
  kind: ConfigMap
  metadata:
    name: app-config
  data:
    APP_COLOR: blue
    APP_MODE: prod
  ```
  - kubectl create -f config-map.yaml
- `kubectl get configmaps`
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
## View Certificate Details:
- `kubectl ...`
## Certificates API:
- `kubectl ...`
## KubeConfig:
- `kubectl ...`
## Role BAsed Access Controls:
- `kubectl ...`
## Cluster Roles:
- `kubectl ...`
## Service Accounts:
- `kubectl ...`
## Image Security:
- `kubectl ...`
## Security Contexts:
- `kubectl ...`
## Network Policies:
- `kubectl ...`
---

# ==Storage==
## Persistent Volume Claims:
- `kubectl ...`
## storage Class:
- `kubectl ...`
---

# ==Networking==
## Explore Environment:
- `kubectl ...`
## CNI:
- `kubectl ...`
## Deploy Network Solution:
- `kubectl ...`
## Networking Weave:
- `kubectl ...`
## Service Networking:
- `kubectl ...`
## CoreDNS in Kubernetes:
- `kubectl ...`
## Ingress Networking - 1:
- `kubectl ...`
## Ingress Networking - 2:
- `kubectl ...`
---

# ==Install==
## cluster Installation using Kubeadm:
- `kubectl ...`
---

# ==Troubleshooting==
## Appliation Failure:
- `kubectl ...`
## Control Plane Failure:
- `kubectl ...`
## Worker Node Failure:
- `kubectl ...`
## Troubleshoot Network:
- `kubectl ...`
---

# ==Other Topics==
## JSON PATH:
- `kubectl ...`
## Advanced Kubectl Commands:
- `kubectl ...`
---

# ==Lighting Labs==
## Ligtning Lab - 1:
- `kubectl ...`
---

# ==Mock Exams==
## Mock Exam - 1:
- `kubectl ...`
## Mock Exam - 2:
- `kubectl ...`
## Mock Exam - 3:
- `kubectl ...`
## CKA Mock Exam - 2 Solution:
- `kubectl ...`
## CKA Mock Exam - 3 Solution:
- `kubectl ...`
---
