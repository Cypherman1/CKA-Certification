# Resource Requirement and Limits

### 1. Resource requests

Default:

* CPU: 0.5
* MEM: 256Mi

Config resource requests in pod or deployment definition

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      resources:
        requests:
          memory: "1Gi"
          cpu: 1
```
{% endcode %}

### 2. Resource limits

Default:

* CPU: 1
* MEM: 512Mi

Config resource limits in pod or deployment definition

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      resources:
        requests:
          memory: "1Gi"
          cpu: 1
        limits:
          memory: "2G"
          cpu: 2
```
{% endcode %}

##
