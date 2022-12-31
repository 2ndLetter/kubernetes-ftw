


# ==Application Lifecycle Management==

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
