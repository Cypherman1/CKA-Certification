# Multiple Schedulers

### 1. Deploy additional scheduler use scheduler's binary

```
wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
```

{% code title="kube-scheduler.service" %}
```
ExecStart=/usr/local/bin/kube-scheduler \\
--config=/etc/kubernetes/config/kube-scheduler.yaml \\
--scheduler-name= default-scheduler
```
{% endcode %}

{% code title="my-custom-scheduler.service" %}
```
ExecStart=/usr/local/bin/kube-scheduler \\
--config=/etc/kubernetes/config/kube-scheduler.yaml \\
--scheduler-name= my-custom-scheduler
```
{% endcode %}

### 2. Deploy additional scheduler use kubeadm

{% code title="/etc/kubernetes/manifests/kube-scheduler.yaml " %}

{% code title="/etc/kubernetes/manifests/kube-scheduler.yaml " %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: kube-scheduler
    namespace: kube-system
spec:
    containers:
    - command:
        - kube-scheduler
        - --address=127.0.0.1
        - --kubeconfig=/etc/kubernetes/scheduler.conf
        - --leader-elect=true
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```
{% endcode %}

{% code title="my-custom-scheduler.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: kube-scheduler
    namespace: kube-system
spec:
    containers:
    - command:
        - kube-scheduler
        - --address=127.0.0.1
        - --kubeconfig=/etc/kubernetes/scheduler.conf
        - --leader-elect=true
        - --scheduler-name=my-custom-scheduler
        - --lock-object-name=my-custom-scheduler
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```
{% endcode %}

### 3. Use scheduler

{% code title="my-custom-scheduler.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx
spec:
    containers:
    - image: nginx
      name: nginx
    schedulerName: my-custom-scheduler
```
{% endcode %}

### 4. Lab

{% code title="my-custom-scheduler.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    component: kube-scheduler
    tier: control-plane
  name: my-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
    - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
    - --bind-address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=false
    - --scheduler-name=my-scheduler
    - --port=10260
    - --secure-port=0
    image: k8s.gcr.io/kube-scheduler:v1.20.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10260
        scheme: HTTPS
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    name: my-scheduler
    resources:
      requests: 
        cpu: 100m
    startupProbe:
      failureThreshold: 24 
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10260
        scheme: HTTPS
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    volumeMounts:
    - mountPath: /etc/kubernetes/scheduler.conf 
      name: kubeconfig
      readOnly: true
  hostNetwork: true
  priorityClassName: system-node-critical
  volumes:
  - hostPath:
      path: /etc/kubernetes/scheduler.conf
      type: FileOrCreate
    name: kubeconfig
status: {}
```
{% endcode %}
