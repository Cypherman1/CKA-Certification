# Rolling Updates and Rollbacks

## Rollout and Versioning

#### Rollout command

```
 kubectl rollout status deployment/myapp-deployment
 kubectl rollout history deployment/myapp-deployment
```

#### Deployment Strategy

* Recreate
* Rolling Update

## Rollback

```
kubectl rollout undo deployment/myapp-deployment
```

## Commands Summary

```
// Create
kubectl create –f deployment-definition.yml
// Get
kubectl get deployments
// Update
 kubectl apply –f deployment-definition.yml
 kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
 // Status
 kubectl rollout status deployment/myapp-deployment
 kubectl rollout history deployment/myapp-deployment
 // Rollback
  kubectl rollout undo deployment/myapp-deployment
```

## Lab

#### RollingUpdate

{% code title="frontend-deployment.yaml" %}
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: webapp
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        imagePullPolicy: IfNotPresent
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP
```
{% endcode %}

#### Recreate

{% code title="frontend-deployment.yaml" %}
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      name: webapp
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        name: webapp
    spec:
      containers:
      - image: kodekloud/webapp-color:v2
        imagePullPolicy: IfNotPresent
        name: simple-webapp
        ports:
        - containerPort: 8080
          protocol: TCP
```
{% endcode %}
