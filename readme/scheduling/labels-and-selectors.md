# Labels and Selectors

### 1. Define `lables` in `metadata` definition

{% code title="pod-definition.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        app: App1
        function: Front-end


spec:
    containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
      - containerPort: 8080  
```
{% endcode %}

### 2. Select

```
kubectl get pods --selector app=App12.3.
```

### 3. Service

{% code title="service-definition.yaml" %}
```yaml
apiVersion: v1
kind: Service
metadata:
    name: my-service
spec:
    selector:
        app: App1
    ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
{% endcode %}

### 4. Replicaset

{% code title="replicaset-definition.yaml" %}
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: simple-webapp
    labels:
        app: App1
        function: Front-end
spec
    replicas: 3
    selector:
        matchLabels:
            app: App1
    template
        metadata:
            labels:
                app: App1
                function: Front-end
        spec:
            containers:
            - name: simple-webapp
              image: simple-webapp
```
{% endcode %}

### 5. Lab

#### Q1

```
kubectl get pod --selector env=dev 
```

#### Q2

```
kubectl get pod --selector bu=finance
```

#### Q3

```
kubectl get all --selector env=prod
```

#### Q4

```
kubectl get pod --selector env=prod,bu=finance,tier=frontend
```

#### Q5

{% code title="replicaset-definition-1.yaml" %}
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
   name: replicaset-1
spec:
   replicas: 2
   selector:
      matchLabels:
        tier: frontend
   template:
     metadata:
       labels:
        tier: frontend
     spec:
       containers:
       - name: nginx
         image: nginx
```
{% endcode %}

##
