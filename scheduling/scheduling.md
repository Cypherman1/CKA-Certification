# Scheduling

## 1. Manual Scheduling

Insert `nodeName` field into Pod `spec` definition.

## 2. Labels and Selectors

### Define `lables` in `metadata` definition

#### **`pod-definition.yaml`**

```
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

### Select

```
kubectl get pods --selector app=App1
```

### Service

#### **`service-definition.yaml`**

```
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

### Replicaset

#### **`replicaset-definition.yaml`**

```
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

### Lab

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

#### **`replicaset-definition-1.yaml`**

```
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

## 3. Taints And Tolerations

#### taint-effect:

* NoSchedule
* PreferNoSchedule
* NoExecute

```
kubectl taint nodes node1 app=blue:NoSchedule
```

### 3.2. Tolerations - PODs

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

### 3.3. Lab

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

## 4. Node Selectors

### 4.1. Define node selectors in pod definition

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

### 4.2. Label Nodes

```
kubectl labekubectl label nodes <node-name> <label-key>=<label-value>
```

```
kubectl label nodes node-1 size=Large
```

## 5. Node Affinity

Ensure pods are hosted in particular nodes with advanced selections

### 5.1. Node Affinity in pod definition

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

### 5.2. Lab

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
