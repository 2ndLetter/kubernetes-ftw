# Backup and Restore Methods:
## Documentation:
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- https://github.com/etcd-io/etcd

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
- Reminder if pod name is appended with "-controlplane", it's a Static Pod

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
- Updated /etc/kubernetes/manifest/etcd.yaml:
```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/etcd.advertise-client-urls: https://10.2.255.9:2379
  creationTimestamp: null
  labels:
    component: etcd
    tier: control-plane
  name: etcd
  namespace: kube-system
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://10.2.255.9:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd # THIS MUST MATCH
    - --experimental-initial-corrupt-check=true
    - --initial-advertise-peer-urls=https://10.2.255.9:2380
    - --initial-cluster=controlplane=https://10.2.255.9:2380
    - --key-file=/etc/kubernetes/pki/etcd/server.key
    - --listen-client-urls=https://127.0.0.1:2379,https://10.2.255.9:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://10.2.255.9:2380
    - --name=controlplane
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    image: k8s.gcr.io/etcd:3.5.3-0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /health
        port: 2381
        scheme: HTTP
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    name: etcd
    resources:
      requests:
        cpu: 100m
        memory: 100Mi
    startupProbe:
      failureThreshold: 24
      httpGet:
        host: 127.0.0.1
        path: /health
        port: 2381
        scheme: HTTP
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    volumeMounts:
    - mountPath: /var/lib/etcd # THIS MUST MATCH
      name: etcd-data
    - mountPath: /etc/kubernetes/pki/etcd
      name: etcd-certs
  hostNetwork: true
  priorityClassName: system-node-critical
  securityContext:
    seccompProfile:
      type: RuntimeDefault
  volumes:
  - hostPath:
      path: /etc/kubernetes/pki/etcd
      type: DirectoryOrCreate
    name: etcd-certs
  - hostPath:
      path: /var/lib/etcd-from-backup
      type: DirectoryOrCreate
    name: etcd-data
status: {}
```