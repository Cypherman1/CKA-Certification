# Static Pods

### 1. Create static pods

Define manifest path in `kubelet.service` file

{% code title="kubelet.service" %}
```
ExecStart=/usr/local/bin/kubelet \\
    --pod-manifest-path=/etc/Kubernetes/manifest \\
    ...
```
{% endcode %}

or

{% code title="kubelet.service" %}
```
ExecStart=/usr/local/bin/kubelet \\
    --config=kubeconfig.yaml \\
    ...
```
{% endcode %}

{% code title="kubeconfig.yaml" %}
```
staticPodPath:/etc/kubernetes/manifest
    ...
```
{% endcode %}

### 2. Lab

{% code title="static-busybox-pod.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - image: busybox
    name: static-busybox
    command: [ "sh", "-ec", "sleep 1000" ]
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
{% endcode %}

