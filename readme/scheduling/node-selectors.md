# Node Selectors

### 1. Define node selectors in pod definition

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
spec:
    containers:
    - name: data-processor
      image: data-processor
    nodeSelector:
      size: Large 
```
{% endcode %}

### 2. Label Nodes

```
kubectl labekubectl label nodes <node-name> <label-key>=<label-value>
```

```
kubectl label nodes node-1 size=Large
```

##
