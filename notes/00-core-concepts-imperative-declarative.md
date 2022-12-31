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
