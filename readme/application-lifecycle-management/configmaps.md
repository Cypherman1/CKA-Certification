# ConfigMaps

## Create ConfigMaps

#### Imperative

```
kubectl create configmap app-config \ 
    --from-literal=APP_COLOR=blue \
    --from-literal=APP_MODE=prod
```

```
kubectl create configmap \
    app-config --from-file=app_config.properties
```

#### Declarative

{% code title="app-config-map.yaml" %}
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_COLOR: blue
    APP_MODE: prod
```
{% endcode %}

## Use ConfigMap in Pods

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      envFrom:
      - configMapRef:
          name: app-config
```
{% endcode %}



{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      env:
      - name: APP_COLOR
        valueFrom:
          configMapKeyRef:
            name: app-config
            key: APP_COLOR
```
{% endcode %}

```yaml
volumes:
    - name: app-config-volume
      configMap:
        name: app-config
```

## LAB

{% code title="webapp-color-pod.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-color
  name: webapp-color
  namespace: default
spec:
  containers:
  - envFrom:
    - configMapRef:
        name: webapp-config-map
    image: kodekloud/webapp-color
    name: webapp-color
```
{% endcode %}
