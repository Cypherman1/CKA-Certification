# Commands and Arguments

## Definition

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: ubuntu-sleeper-pod
spec:
    containers:
    - name:
      image: ubuntu-sleeper
      command: ["sleep2.0"]
      args: ["10"]
```
{% endcode %}

## LAB

