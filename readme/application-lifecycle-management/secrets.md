# Secrets

## Create Secrets

#### Imperative

```
kubectl create secret generic \
    app-secret --from-literal=DB_Host=mysql
               --from-literal=DB_User=root
               --from-literal=DB_Password=paswrd
```

```
kubectl create secret generic \
    app-secret --from-file=app_secret.properties
```

#### Declarative

{% code title="secret-data.yaml" %}
```yaml
apiVersion: v1
kind: Secret
metadata:
    name: app-secret
data:
    DB_Host: bXlzcWw=
    DB_User: cm9vdA==
    DB_Password: cGFzd3Jk
```
{% endcode %}

encode

```bash
echo -n 'mysql' | base64
echo -n 'root' | base64
echo -n 'paswrd' | base64
```



decode

```bash
echo -n 'bXlzcWw=' | base64 --decode
echo -n 'cm9vdA==' | base64 --decode
echo -n 'cGFzd3Jk' | base64 --decode
```

## Use Secrets in Pods

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
      - secretRef:
          name: app-secret
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
      - name: DB_Password
        valueFrom:
          secretKeyRef:
            name: app-secret
            key: db_Password
```
{% endcode %}

```yaml
volumes:
    - name: app-secret-volume
      secret:
        secretName: app-secret
```

```bash
ls /opt/app-secret-volumes
DB_Host DB_Password DB_User

cat /opt/app-secret-volumes/DB_Password
paswrd
```

## LAB

{% code title="db-secret.yaml" %}
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
data:
  DB_Host: c3FsMDE=
  DB_User: cm9vdA==
  DB_Password: cGFzc3dvcmQxMjM=
```
{% endcode %}

{% code title="web-app-pod.yaml" %}
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    name: webapp-pod
  name: webapp-pod
  namespace: default
spec:
  containers:
  - image: kodekloud/simple-webapp-mysql
    imagePullPolicy: Always
    name: webapp
    envFrom:
    - secretRef:
            name: db-secret
```
{% endcode %}

##
