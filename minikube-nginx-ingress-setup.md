# 1️⃣ Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
kubectl version --client

# 2️⃣ Install Minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# 3️⃣ Start Minikube
minikube start
minikube status

# 4️⃣ Enable Ingress addon
minikube addons enable ingress
kubectl get pods -n ingress-nginx

# 5️⃣ Create first deployment (web v1)
kubectl create deployment web --image=gcr.io/google-samples/hello-app:1.0
kubectl get deployment web

# 6️⃣ Expose deployment as NodePort
kubectl expose deployment web --type=NodePort --port=8080
kubectl get svc
minikube service web --url

# 7️⃣ Create second deployment (web2 v2)
kubectl create deployment web2 --image=gcr.io/google-samples/hello-app:2.0
kubectl get deployment web2
kubectl expose deployment web2 --port=8080 --type=NodePort
kubectl get svc
minikube service web2 --url

# 8️⃣ Test service via curl (with Minikube IP)
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example/v2

# 9️⃣ Ingress setup
kubectl describe ingress
vim ingress.yaml  # create/edit your ingress YAML
kubectl apply -f ingress.yaml
kubectl get ingress

# 🔟 Test Ingress routes
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example
curl --resolve "hello-world.example:80:$(minikube ip)" -i http://hello-world.example/v2
curl http://hello-world.example

