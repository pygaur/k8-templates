# Persistent Redis setup inside local kubernetes using minikube
## We will deploy single instance(Pod) of Redis using PersistentVolume and PersistentVolumeClaim
1. Check if ingress is enabled in minikube or not.<br/>It will help to route traffic from their host (Laptop, Desktop, etc) to a Kubernetes service running inside their minikube cluster.
- To check if ingress is enabled - 
<br/>`minikube addons list | grep 'ingress'`
- To enable ingress -
<br/>`minikube addons enable ingress`
2. Setup ingress for TCP
```
kubectl patch configmap tcp-services -n ingress-nginx --patch "$(cat ingress-nginx-patch.yaml)"
```
Confirm if patch applied successfully --> 
```bash
kubectl get configmap tcp-services -n ingress-nginx -o yaml | grep '6379'
```
Output should looks like this:
> `"6379": default/redis-service:6379`

3. Patch the ingress-nginx-controller 
<br/>We need to patch our nginx controller so that it is listening on port 6379 and can route traffic to service.
```
kubectl patch deployment ingress-nginx-controller --patch "$(cat ingress-nginx-controller-patch.yaml)" -n ingress-nginx
```

4. Create PersistentVolume 
```
kubectl apply -f redis-pv.yaml
```

5. Create PersistentVolumeClaim 
```
kubectl apply -f redis-pvc.yaml
```

6. Create Deployment
```
kubectl apply -f redis-deployment.yaml
```

7. Create Service
```
kubectl apply -f redis-service.yaml
```
