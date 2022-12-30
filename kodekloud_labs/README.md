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
- `kubectl get pod --selector env=dev`
- `kubectl get pod --selector bu=finance`
- `kubectl get pod --selector bu=finance --no-headers | wc -l` # Returns number of Objects
- `kubectl get all --selector env=prod,bu=finance,tier=frontend`
- `kubectl apply -f replicaset-definition-1.yaml`
  - Fixed replicaset-definition-1.yaml
  - Selector matchLabels didn't match Pod's label
  - Run `k apply ....` again
## Taints and Tolerations:
- `kubectl taint nodes <node-name> <key=value>:<taint-effect>` # Example syntax
  - taint-effect: NoSchedule | PreferNoSchedule | NoExecute
- `kubectl taint nodes node1 app=blue:NoSchedule`
- `kubectl describe node kubemaster | grep Taint` # Review taints on the Master node
- `kubectl taint nodes node01 spray=mortein:NoSchedule`
- `kubectl run bee --image=nginx --dry-run=client -o yaml > bee.yaml`
  - Add "tolerations" section
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: bee
      name: bee
    spec:
      containers:
      - image: nginx
        name: bee
      tolerations:
      - key: "spray"
        operator: "Equal"
        value: "mortein"
        effect: "NoSchedule"  
    ```
  - `kubectl apply -f bee.yaml`
  - `kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-`
    - This removes the default taint on the controlplane node
## Node Affinity:
- Learn to manipulate multiple lines via VIM, improper spacing causes problems
  - Shift + V
  - Shift + >
- `kubectl label node node01 color=blue`
- `kubectl create deployment blue --image=nginx --replicas=3 --dry-run=client -o yaml > blue.yaml`
  - `kubectl apply -f blue.yaml`
- `kubectl describe node controlplane | grep Taint`
- Manifest files used:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: blue
    name: blue
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: blue
    strategy: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: blue
      spec:
        containers:
        - image: nginx
          name: nginx
          resources: {}
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: color
                  operator: In
                  values:
                  - blue
  ```
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    creationTimestamp: null
    labels:
      app: red
    name: red
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: red
    strategy: {}
    template:
      metadata:
        creationTimestamp: null
        labels:
          app: red
      spec:
        containers:
        - image: nginx
          name: nginx
          resources: {}
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: node-role.kubernetes.io/control-plane
                  operator: Exists
  
  status: {}
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
## DaemonSets:
- `kubectl get daemonset -A -o wide`
- `kubectl describe ds kube-proxy -n kube-system`
- `kubectl create deplyment elasticsearch -n kube-system --image=k8s.gcr.io/fluentd-elasticsearch:1.20 --dry-run=client -o yaml > fluentd.yaml`
- Change kind to DaemonSet, remove replicas
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: elasticsearch
  namespace: kube-system
spec:
  selector:
    matchLabels:
      name: elasticsearch
  template:
    metadata:
      labels:
        name: elasticsearch
    spec:
      containers:
      - name: elasticsearch
        image: k8s.gcr.io/fluentd-elasticsearch:1.20
```
- `kubectl apply -f fluentd.yaml`
## Static PODs:
- How to find path to Static Pods:
  - Default directory "/etc/kubernetes/manifests"
  - kubelet.service config file:
    - "--pod-manifest-path=<path/to/manifests>"
    - OR
    - provide a path to using "--config=kubeconfig.yaml"
  - kubeconfig.yaml: "staticPodPath: <path/to/manifests>"
- `docker ps` # Use to see running pods
- `kubectl ...`
- `ps -aux | grep kubelet > output`
  - vim output
  - search for "--config"
- `cat /var/lib/kubelet/config.yaml | grep static`
- `cat /var/kubernetes/manifest/kube-apiserver.yaml | grep image`
- `kubectl run static-busybox --image=busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static.yaml`
- Update the image within the static.yaml
- `kubectl delete po static-busybox-controlplane`
- `k get nodes -o wide`
- `ssh <nod01_ip_address>`
- `ps -aux | grep kubelet`
- `cat /var/lib/kubelet/config.yaml | grep static`
- Remove file in the identified directory
## Multiple Schedulers:
- `kubectl get pods -A`
- `kubectl describe pod kube-scheduler-controlplane -n kube-system`
- `kubectl get sa my-scheduler`
- `kubectl create configmap my-scheduler-config --from-file=/root/my-scheduler-config.yaml -n kube-system`
- `kubectl get configmap my-scheduler-config -n kube-system`
---

# ==Logging & Monitoring==
## Monitor Cluster Components:
- `git clone https://github.com/kubernetes-incubator/metrics-server.git`
- kubectl apply -f deploy/1.8+/
- `kubectl top pod`
- `kubectl top node`
## Managing Application Logs:
- `docker logs -f ecf`
- `kubectl top node`
- `kubectl logs -f pod webapp-1`
- If multiple containers within a Pod, you must provide the Pod and Container name
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
