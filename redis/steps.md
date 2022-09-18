# &lt;clipboard-copy&gt; element

# Persistent Redis setup inside local kubernetes using minikube
## We will deploy single instance(Pod) of Redis using PersistentVolume and PersistentVolumeClaim
* Check if ingress is enabled in minikube or not.<br/>It will help to route traffic from their host (Laptop, Desktop, etc) to a Kubernetes service running inside their minikube cluster.
<br/>1. To check if ingress is enabled - 
<br/>`minikube addons list | grep 'ingress'`
<br/>2. To enable ingress -
<br/>`minikube addons enable ingress`
* Setup ingress for TCP
```
kubectl patch configmap tcp-services -n ingress-nginx --patch "$(cat ingress-nginx-patch.yaml)"
```
Confirm if patch applied successfully --> 
```bash
kubectl get configmap tcp-services -n ingress-nginx -o yaml | grep '6379'
```
Output should looks like this:
```
  "6379": default/redis-service:6379
```
* Patch the ingress-nginx-controller 
<br/>We need to patch our nginx controller so that it is listening on port 6379 and can route traffic to your service.
```
kubectl patch deployment ingress-nginx-controller --patch "$(cat ingress-nginx-controller-patch.yaml)" -n ingress-nginx
```
* Create PersistentVolume 
```
kubectl apply -f redis-pv.yaml
```
* Create PersistentVolumeClaim 
```
kubectl apply -f redis-pvc.yaml
```
* Create deployment
```
kubectl apply -f redis-deployment.yaml
```
* Create service
```
kubectl apply -f redis-service.yaml
```
