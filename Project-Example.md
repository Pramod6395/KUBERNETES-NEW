
#### Main Components:

![image](https://user-images.githubusercontent.com/73251890/234183075-7d72799e-c55d-4150-918f-4380301b4dbc.png)

1. Mongo Express : is web application which receive request via Extrenal Service
2. MongoDB: is database which will receive request from Mongo express and response only when credential are correct.
3. Configmap: this will have Database URL which will required to Mongo Express to connect to DB
4. Secrets: This will have DB username and password which will required to Mongo Express to connect to DB
5. External Service: This will receive request from User then directed toword Mongo express
6. Internal Service: This will receive request from Mongo Express and then directed to mongoDB

### Architecture

![image](https://user-images.githubusercontent.com/73251890/234183254-f8d46bb6-bb97-4fa5-852f-36b0f83f634b.png)

### 1. Copy below containt and Create file name mongo.yaml
~~~bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
  labels:
    app: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:latest
        ports:
        - containerPort: 27017
        env:
        - name: MONGO_INITDB_ROOT_USERNAME
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: MONGO_INITDB_ROOT_PASSWORD
          valueFrom: 
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
---
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017

~~~
### 2. Copy below containt and create file mongo-express.yaml

~~~bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo-express
  template:
    metadata:
      labels:
        app: mongo-express
    spec:
      containers:
      - name: mongo-express
        image: mongo-express
        ports:
        - containerPort: 8081
        env:
        - name: ME_CONFIG_MONGODB_ADMINUSERNAME
          valueFrom:
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-username
        - name: ME_CONFIG_MONGODB_ADMINPASSWORD
          valueFrom: 
            secretKeyRef:
              name: mongodb-secret
              key: mongo-root-password
        - name: ME_CONFIG_MONGODB_SERVER
          valueFrom: 
            configMapKeyRef:
              name: mongodb-configmap
              key: database_url
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-express-service
spec:
  selector:
    app: mongo-express
  type: LoadBalancer  
  ports:
    - protocol: TCP
      port: 8081
      targetPort: 8081
      nodePort: 30000
~~~

### 3. Copy containt below and create file mongo-configmap.yaml
~~~bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb-service
~~~
### 4. Copy below containt and create file mongo-secret.yaml

~~~bash
apiVersion: v1
kind: Secret
metadata:
    name: mongodb-secret
type: Opaque
data:
    mongo-root-username: dXNlcm5hbWU=
    mongo-root-password: cGFzc3dvcmQ=
~~~
Note that secret are kept in base64 encoded not in plane text so you need to generate that using below command on linux terminal
~~~bash
echo -n 'username' | base64
~~~
~~~bash
echo -n 'password' | base64
~~~
Put encoded text in above file for mongo-root-usernam and  mongo-root-password

### 4. Deploy all resources using below command.
~~~bash
kubectl create -f mongo-secret.yaml
kubectl create -f mongo-configmap.yaml
kubectl create -f mongo.yaml
kubectl create -f mongo-express.yaml
~~~
Note: order should be same to execute above command.
### 5. Check our deployment and pods
~~~bash
kubectl get deployment
kubectl get pods
kubectl get secret
kubectl get configmap
~~~
### 6. Access the Mongo express using browser you need to check IP of minikube.
~~~bash
minikube service list
~~~
![image](https://user-images.githubusercontent.com/73251890/234200450-71bfb9de-b5f1-4cc1-a547-79d05de6f746.png)

Now just click on URL  http://192.168.49.2:30000 you will see web page for mongo-express.


![image](https://user-images.githubusercontent.com/73251890/234200885-06830218-23ce-4f37-9f1e-ec7a7d38a726.png)

