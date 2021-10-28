# Scheduling

## 1. Manual Scheduling

Insert `nodeName` field into Pod `spec` definition.

## 2. Labels and Selectors

### 2.1. Define `lables` in `metadata` definition

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

### 2.2. Select

```
kubectl get pods --selector app=App12.3.
```

### 2.3. Service

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

### 2.4. Replicaset

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

### 2.5. Lab

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

## 3. Taints And Tolerations

### 3.1. Taints - Node

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

## 6. Resource Requirement and Limits

### 6.1. Resource requests

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

### 6.1. Resource limits

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

## 6. Daemon Sets

### 6.1. DaemonSet definition

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
### 6.2. Lab

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
## 7. Static Pods
### 7.1. Create static pods
Define manifest path in `kubelet.service` file

{% code title="kubelet.service" %}
```
ExecStart=/usr/local/bin/kubelet \\
    --pod-manifest-path=/etc/Kubernetes/manifest \\
    ...
```
{% endcode %}

or

{% code title="kubelet.service" %}
```
ExecStart=/usr/local/bin/kubelet \\
    --config=kubeconfig.yaml \\
    ...
```
{% endcode %}

{% code title="kubeconfig.yaml" %}
```
staticPodPath:/etc/kubernetes/manifest
    ...
```
{% endcode %}

### 7.2. Lab

{% code title="static-busybox-pod.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: static-busybox
  name: static-busybox
spec:
  containers:
  - image: busybox
    name: static-busybox
    command: [ "sh", "-ec", "sleep 1000" ]
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
{% endcode %}

