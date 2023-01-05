# Backup and Restore Methods 2:

## Notes:
- tbd

## KodeKloud Lab:
- `kubectl get deploy`
- `kubectl get svc`
- `kubectl describe -n kube-system pod etcd-controlplane`
- Copy etcd pod into a file called "notes":
  ```
  --data-dir=/var/lib/etcd
  --cert-file=/etc/kubernetes/pki/etcd/server.crt
  --key-file=/etc/kubernetes/pki/etcd/server.key
  --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
  ```
- `cat notes` # Easily view information
- Backup etcd:
  - `export ETCDCTL_API=3` # Creat var
  - `etcdctl snapshot save /opt/snapshot-pre-boot.db --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key`
- Restore etcd:
  - `etcdctl --data-dir /var/lib/etcd-from-backup snapshot restore /opt/snapshot-pre-boot.db --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key`
- Edit etcd Static Pod:
  - `vim /etc/kubernetes/manifest/etcd.yaml`
  - Change the path of the volume's etcd-data to the restored location:
    ```
    path: /var/lib/etcd-from-backup
    ```
  - `watch "crictl ps | grep etcd"` # Wait for etcd pod to come back online
  - Delete etcd-controlplane pod if it doesn't come up properly
- Verify Deployments and Services are online:
  - `kubect get deploy,svc`
- kubectl config --help:
```
Available Commands:
  current-context Display the current-context
  delete-cluster  Delete the specified cluster from the kubeconfig
  delete-context  Delete the specified context from the kubeconfig
  delete-user     Delete the specified user from the kubeconfig
  get-clusters    Display clusters defined in the kubeconfig
  get-contexts    Describe one or many contexts
  get-users       Display users defined in the kubeconfig
  rename-context  Rename a context from the kubeconfig file
  set             Set an individual value in a kubeconfig file
  set-cluster     Set a cluster entry in kubeconfig
  set-context     Set a context entry in kubeconfig
  set-credentials Set a user entry in kubeconfig
  unset           Unset an individual value in a kubeconfig file
  use-context     Set the current-context in a kubeconfig file
  view            Display merged kubeconfig settings or a specified kubeconfig file

Usage:
  kubectl config SUBCOMMAND [options]
```
