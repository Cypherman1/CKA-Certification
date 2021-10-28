# Daemon Sets

### 1. DaemonSet definition

{% code title="daemon-set-definition.yaml" %}
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent
```
{% endcode %}

### 2. Lab

{% code title="daemon-set-definition.yaml" %}
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
   labels:
     app: elasticsearch
   name: elasticsearch
   namespace: kube-system
spec:
   selector:
     matchLabels:
       app: elasticsearch
   template:
     metadata:
       labels:
         app: elasticsearch
     spec:
       containers:
       - image: k8s.gcr.io/fluentd-elasticsearch:1.20
         name: fluentd-elasticsearch
```
{% endcode %}

##
