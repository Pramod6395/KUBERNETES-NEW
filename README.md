# KUBERNETES-NEW

Create pod using manually created images:
```bash
kubectl run [POD-NAME] --image=[IMAGE NAME] --image-pull-policy=IfNotPresent

```
Create pod using yaml file with own image
save this file as pod.yaml
```bash
apiVersion: v1
kind: Pod
metadata:
  name: myapp
  labels:
    app: myapp
spec:
  containers:
    - name: myapp
      imagePullPolicy: IfNotPresent
      image: send_telemetry
      ports:
        - containerPort: 8080
```

Create pod :
```bash
kubectl create -f pod.yaml

```
