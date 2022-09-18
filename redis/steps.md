1. minikube addons enable ingress
2. kubectl apply -f redis-deployment.yaml
3. kubectl apply -f redis-service.yaml
4. kubectl patch configmap tcp-services -n ingress-nginx --patch "$(cat ingress-nginx-patch.yaml)"
5. kubectl get configmap tcp-services -n ingress-nginx -o yaml
6. s
