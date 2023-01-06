# Section Title:
## Documentation:
- https://kubernetes.io/docs/home/

## Usage:
- Imperative Way:
  - `kubectl ...`
- Delcarative Way:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
```
## Commands:
- `kubectl get ...`
- `kubectl describe ...`

## Notes:
### Authentication Mechanisms: (kube-apiserver)
  ### Static Password File:
  - user-details.csv:
  ```
  password123,user1,u0001,group1
  password123,user2,u0002,group1
  password123,user3,u0003,group2
  ```
  - If NOT running in a Pod: (kube-apiserver.service)
  ```
  ExecStart=/usr/local/bin/kube-apiserver \\
  ...
  --basic-auth-file=user-details.csv
  ...
  ```
  - Restart kube-server for changes to take affect
  - If running in a Pod:
  ```
  ...
  spec:
    containers:
    - command:
      - --basic-auth-file=user-details.csv
      ...
  ```
  - To authenticate using basic credentials:
    - `curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`
  ---
  ### Static Token File:
  - user-token-details.csv:
  ```
  LKIJ3kj3l4kLKJ3kjLk4k3kkdfls,user10,u0010,group1
  LKIJ3kj4k3kkdfls3l4kLKJ3kjLk,user11,u0011,group1
  l4kLKJ3kjLk4kLKIJ3kj33kkdfls,user12,u0012,group2
  ```
  - Use this option: `--token-auth-file=user-details.csv`
  - To authenticate using a token file:
    - `curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer LKIJ3kj3l4kLKJ3kjLk4k3kkdfls"`
  ---
  - IMPORTANT NOTES:
    - Static Password/Token file is NOT recommended
    - Consider volume mount while providing the auth file in kubeadm setup
    - Setup RBAC for new users
    - Basic Authentication was deprecated in 1.19 (2020-08-26)
  ### Certificates:
  ### Identity Services:

