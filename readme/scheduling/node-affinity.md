# Node Affinity

Ensure pods are hosted in particular nodes with advanced selections

### 1. Node Affinity in pod definition

```yaml
apiVersion: 
kind:
metadata:
    name: myapp-pod
spec:
    containers:
    - name: data-processor
      image: data-processor
    affinity:
        nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution/preferredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                    - key: size
                      operator: In/NotIn/Exists(no need values)
                      values:
                      - Large
```

### 2. Lab

#### Q6

{% code title="blue-deployment.yaml" %}
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: blue
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: blue
  template:
    metadata:
      labels:
        app: blue
    spec:
      containers:
      - image: nginx
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
```
{% endcode %}

#### Q8

{% code title="red-deployment.yaml" %}
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: red
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      app: red
  template:
    metadata:
      labels:
        app: red
    spec:
      containers:
      - image: nginx
        name: nginx
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/master
                operator: Exists
```
{% endcode %}

