# Monitor cluster components

### Install Metrics Server

#### minikube

```
minikube addons enable metrics-server
```

#### others

```
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create –f deploy/1.8+/
```

```
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
cd kubernetes-metrics-server
kubectl create -f .
```

### View&#x20;

```
kubectl top node
```

```
kubectl top pod
```
