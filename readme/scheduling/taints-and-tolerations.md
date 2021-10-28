# Taints And Tolerations

### 1. Taints - Node

```
kubectl taint nodes node-name key=value:taint-effect
```

#### taint-effect:

* NoSchedule
* PreferNoSchedule
* NoExecute

```
kubectl taint nodes node1 app=blue:NoSchedule
```

### 2. Tolerations - PODs

```yaml
apiVersion: v1 
kind: Pod
metadata:
    name: myapp-pod
spec:
    containers:
    - name: nginx-container
      image: nginx
    tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
```

### 3. Lab

#### Q7

{% code title="bee-pod.yaml" %}
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
    value: "mortein"
    effect: "NoSchedule"
```
{% endcode %}

##
