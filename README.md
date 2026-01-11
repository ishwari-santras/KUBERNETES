# Pod.yaml
````
apiVersion: v1
kind: Pod
metadata:
  name: k8s
  labels:
    app: k8s-app
spec:
  containers:
    - name: c1
      image: ishwarisantras/devops-b2:piano
      ports:
       - containerPort: 80
````
# Service.yaml
````
apiVersion: v1
kind: Service
metadata:
  name: svc-k8s
spec:
  selector:
    app: k8s-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
````
# Deployment.yaml
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rc-games
spec:
  replicas: 3
  selector:
    matchLabels:
     env: dev
  template:
    metadata:
      name: tmp-game
      labels:
        env: dev
    spec:
      containers:
      - name: c1
        image: ishwarisantras/devops-b2:piano
        ports:
        - containerPort: 80
````
# Service.yaml
````
apiVersion: v1
kind: Service
metadata:
  name: svc-game
spec:
  selector:
    env: dev
  ports:
   - protocol: TCP
     port: 80
     targetPort: 80
  type: NodePort
````
# Replication Set
````
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rc-games
spec:
  replicas: 3
  selector:
    matchLabels:
     env: dev
  template:
    metadata:
      name: tmp-game
      labels:
        env: dev
    spec:
      containers:
      - name: c1
        image: ishwarisantras/devops-b2:piano
        ports:
        - containerPort: 80
````
# Replication Controller
````
apiVersion: v1
kind: ReplicationController
metadata:
  name: vc-game
spec:
  replicas: 4
  selector:
    env: dev
  template:
    metadata:
      name: tmp-game
      labels:
        env: dev
    spec:
      containers:
        - name: c1
          image: ishwarisantras/devops-b2:piano
          ports:
            - containerPort: 80
