# Kubernetes Single YAML Deployment
This repository contains a **single Kubernetes YAML file** defining multiple resources.

## Resources Included
1. Secret (app-secrets)
2. ConfigMap (my-configmap)
3. Deployment (my-app-deployment)
4. Service (my-app-service)

All resources are defined in one file using `---` separators.

## create configmap
````
vim app.yaml
````
apiVersion: v1
kind: Secret
metadata:
  name: app-secrets
type: Opaque
data:
  DB_PASSWORD: UGFzc3dvcmRAMTIz   # base64 of "Password@123"

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  DB_URL: mydbinstance.c6c8dfghilnt.us-east-1.rds.amazonaws.com
  DB_USERNAME: admin

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: nginx
          ports:
            - containerPort: 80
          env:
            - name: DB_URL
              valueFrom:
                configMapKeyRef:
                  name: my-configmap
                  key: DB_URL
            - name: DB_USERNAME
              valueFrom:
                configMapKeyRef:
                  name: my-configmap
                  key: DB_USERNAME
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: app-secrets
                  key: DB_PASSWORD

---
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80          # Service port
      targetPort: 80    # Container port
      nodePort: 30080   # NodePort (30000–32767)
````
kubectl apply -f app.yaml
````
kubectl get secret
kubectl get configmap
kubectl get deployment
kubectl get pods
kubectl get svc
````
kubectl get pods

Pod status should be:RUNNING
````
kubectl exec -it <podname> --/bin/sh
````
kubectl get nodes -o wide
````
curl http://<IP>:30080
````
