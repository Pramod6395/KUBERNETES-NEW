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

Create the Deployment by running the following command:
```bash
kubectl apply -f deployment.yaml
```
Check if the Deployment was created.
```bash
kubectl get deployments
```
To see the Deployment rollout status
```bash
kubectl rollout status deployment/nginx-deployment

```
To see the ReplicaSet
```bash
kubectl get rs
```
To see the labels automatically generated for each Pod

```bash
kubectl get pods --show-labels
```
Updating a Deployment, let's update the nginx Pods to use the nginx:1.16.1 image instead of the nginx:1.14.2 image.

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1
```
Alternatively, you can edit the Deployment and change

```bash
kubectl edit deployment/nginx-deployment
```
Get details of your Deployment

```bash
kubectl describe deployments
```
Checking Rollout History of a Deployment

```bash
kubectl rollout history deployment/nginx-deployment
```
To see the details of each rollout revision, run

```bash
kubectl rollout history deployment/nginx-deployment --revision=2
```
Rolling Back to a Previous Revision
```bash
kubectl rollout undo deployment/nginx-deployment
```
Rollback to a specific revision
```bash
kubectl rollout undo deployment/nginx-deployment --to-revision=2
```
Scale a Deployment
```bash
kubectl scale deployment/nginx-deployment --replicas=10
```
Setup an autoscaler for your Deployment and choose the minimum and maximum number of Pods you want to run based on the CPU utilization of your existing Pods.
```bash
kubectl autoscale deployment/nginx-deployment --min=10 --max=15 --cpu-percent=80
```
Check the current status of the newly-made HorizontalPodAutoscaler (hpa)
```bash
kubectl get hpa
```
Delete HorizontalPodAutoscaler(hpa)
```bash
kubectl delete hpa NAME-OF-HPA
```
Update k8s ConfigMap or Secret without deleting the existing one
```bash
kubectl create configmap config-tel --from-file=/home/pramod/Desktop/Nodebook/project/new_docker/app/ -o yaml --dry-run | kubectl apply -f -

```
Delete deployments
```bash
kubectl delete deploy <deployment name>

```
To enter into to the pods console
```bash
 kubectl exec -it <pod name> bin/bash
```

To set default namespace
```bash
 kubectl config set-context --current --namespace=NAMESPACE
```
Minikube dashboard with matrics
```bash
minikube addons enable metrics-server
minikube dashboard --url
```

To check envirment variable of any pod
```bash
 kubectl exec PODNAME -- printenv
```
Port forwarding for specific service.
```bash
 kubectl port-forward --address 0.0.0.0 services/[service-name] [extrenalport]:[service-internalport]
 kubectl port-forward --address 0.0.0.0 services/tb-node 8080:8080
```
Port forwarding for specific service in baground.
```bash
 kubectl port-forward --address 0.0.0.0 services/[service-name] [extrenalport]:[service-internalport] &
 kubectl port-forward --address 0.0.0.0 services/tb-node 8080:8080 &
```
To display the linux process and kill the pid.
```bash
netstat -tulpn
kill <pid>
```

To check CPU and RAM consumation of any pod.
```bash
kubectl top pod [pod name]
```

To enter into specific container in pod [Multicontainer Pod].
```bash
kubectl exec -it [pod name] -c [container name ] bash
```

Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

