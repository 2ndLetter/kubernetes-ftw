## Multi Profile Scheduler
```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
  plugins:
    score:
      disabled:
        - name: TaintToleration
        enabled:
        - name: MyCustomPluginA
        - name: MyCustomPluginB
- schedulerName: my-scheduler-3
  plugins:
    preScore:
      disabled:
        - name: '*'
    score:
      disabled:
        - name: '*'
```

## Multiple Schedulers:
- `kubectl get pods -A`
- `kubectl describe pod kube-scheduler-controlplane -n kube-system`
- `kubectl get sa my-scheduler`
- `kubectl create configmap my-scheduler-config --from-file=/root/my-scheduler-config.yaml -n kube-system`
- `kubectl get configmap my-scheduler-config -n kube-system`
---