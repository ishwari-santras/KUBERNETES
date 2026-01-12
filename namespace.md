# NameSpace
````
A Namespace is a logical partition of a kubernetes cluster it used to seperate resources like pods,services,deployments for different environment.
````
#### 1. To create namespace(ns)(Imparative) #### 
````
kubectl create ns prod 
 OR
kubectl create namespace prod 
````
#### 2. To check namespace ####
````
kubectl get ns
 OR
kubectl get namespace
````
#### 3. view pods #### 
````
kubectl get pods
````
#### 4. To check object version #### 
````
kubectl api-resources
````
#### 5. To create pods in specific ns ####
````
kubectl run my-pod --image=ishwarisantras/devops-b2:v1
````
#### 6. To delete pods ####
````
kubectl delete pod (pod name)
````
#### 7. To delete deploy file ####
````
kubectl delete deploy (deploy name)
````
#### 8. To check deploy ####
````
kubectl get deploy
````
#### 9. To create and apply namespace ####
````
kubectl apply -f deploy.yaml -n prod
````
#### 10. To check pods in specific ns ####
````
kubectl get pods -n prod
````
#### 11. To check deploy in created in prod ####
````
kubectl get deploy -n prod
````
#### 13. To switch another created namespace ####
````
kubectl config set-context --current --namespace=test
````
#### 14. To check current login namespace ####
````
kubectl config view --minify | grep namespace: 
````
#### 15. To edit in deploy.yaml ####
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rc-games
  namespace: ecom
spec:
  replicas: 3
  selector:
    matchLabels:
     app: blue
  template:
    metadata:
      name: tmp-game
      labels:
        app: blue
    spec:
      containers:
      - name: c1
        image: ishwarisantras/devops-b2:blue
        ports:
        - containerPort: 80
````
#### 16. To apply ####
````
kubectl apply -f deploy.yaml
````
# Namespace File (Declarative)
````
apiVersion: v1
kind: Namespace
metadata:
 name: dev
````
#### 17. To check running pods ####
````
kubectl get pods --all-namespace
````
#### 18. TO check running deploy ####
````
kubectl get deploy --all-namespace
````
#### 19. To check svc in namespace ####
````
kubectl get svc -n test
````
      
