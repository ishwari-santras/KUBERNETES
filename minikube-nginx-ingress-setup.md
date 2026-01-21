````
#### Install kubectl
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
kubectl version --client

# Install Minikube
````
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# Start Minikube
````
minikube start
minikube status

# Enable Ingress addon
````
minikube addons enable ingress
kubectl get pods -n ingress-nginx

# Create first deployment (web v1)
````
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
kubectl get deployment web

# Expose deployment as NodePort
````
kubectl expose deployment web --type=NodePort --port=8080
kubectl get svc
minikube service web --url

# Create second deployment (web2 v2)
````
kubectl create deployment web2 --image=gcr.io/google-samples/hello-app:2.0
kubectl get deployment web2
kubectl expose deployment web2 --port=8080 --type=NodePort
kubectl get svc
minikube service web2 --url
``
# vim ingress.yaml
``
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  ingressClassName: nginx
  rules:
    - host: hello-world.example
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web
                port:
                  number: 8080
          - path: /v2
            pathType: Prefix
            backend:
              service:
                name: web2
                port:
                  number: 8080

``
kubectl apply -f ingress.yaml
``
kubectl get ingress

# Test Ingress routes
````
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example/v2

