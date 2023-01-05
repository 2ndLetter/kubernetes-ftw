# Backup and Restore Methods:
## Documentation:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- https://github.com/etcd-io/etcd

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
- `kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml` # Backups the resources as yaml
- `export ETCDCTL_API=3` # Set variable
- `ETCDCTL_API=3 etcdctl snapshot save </path-to-new-snapshot/>snapshot.db`
- `ETCDCTL_API=3 etcdctl snapshot status </path-to-new-snapshot/>snapshot.db`
- Restore snapshot:
  - `service kube-apiservice stop`
  - `ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-from-backup`
  - Configure etcd.service to use new data directory:
    - `--data-dir=/var/lib/etcd-from-backup`
  - `systemctl daemon-reload`
  - `systemctl restart etcd`
  - `service kube-apiserver start`
- For each etcdctl commands add the below options:
  ```
  ETCDCTL_API=3 etcdctl \
    snapshot save snapshot.db \
    --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/etcd/ca.crt \
    --cert=/etc/etcd/etcd-server.crt \
    --key=/etc/etcd/etcd-server.key
  ```
## Notes:
- Store manifests in GitHub (SCM)
- Backup the actual directory where etcd.service keeps its data (--data-dir=/var/lib/etcd)

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
- Verify Deployments and Services are online:
  - `kubect get deploy,svc`
