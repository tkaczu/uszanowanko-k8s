UP #28: Kubernetes - from zero to be hero
=============

Repozytorium zawiera snipety prezentowane na Uszanowanko Programowanko #28 [https://uszanowanko.pl/meetup/28-z-pipelinami-w-chmurze]

# k8s - GKE uwierzytelnienie
```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project <PROJECT ID>
gcloud container clusters get-credentials <CREDENTIALS> --zone us-central1-a --project <PROJECT ID>
```

# Nginx - intro
```bash
cd nginx-intro

kubectl get pods -o wide
kubectl get svc

kubectl apply -f deployment.yaml
kubectl get pods -o wide

kubectl expose deployment nginx-intro --type=LoadBalancer --name=nginx-intro
kubectl get svc
kubectl describe svc nginx-intro

kubectl get pods -o wide
kubectl delete pod <NAZWA_PODA> 
kubectl delete svc nginx-intro

kubectl get pods -o wide
kubectl get deployments
kubectl delete deployment nginx-intro
kubectl get pods -o wide
```

# Wordpress - single
```bash
cd wordpress-single

kubectl apply -f namespace.yaml -f deployment.yaml -f service.yaml

kubectl get pods -o wide -n up-k8s-example-two
kubectl get svc -o wide -n up-k8s-example-two

kubectl get deployments -n up-k8s-example-two
kubectl delete deployment wordpress-single -n up-k8s-example-two
kubectl delete svc wordpress-single -n up-k8s-example-two
```

# Wordpress - multi
```bash
cd wordpress-multi

kubectl apply -f namespace.yaml -f configmap.yaml 
kubectl apply -f mysql-deployment.yaml -f mysql-service.yaml
kubectl apply -f wp-deployment.yaml -f wp-service.yaml

kubectl get pods -o wide -n up-k8s-example-three
kubectl get svc -o wide -n up-k8s-example-three
```

# Wordpress - multi-hpa
```bash
cd wordpress-multi-hpa

kubectl apply -f namespace.yaml -f configmap.yaml 
kubectl apply -f mysql-deployment.yaml -f mysql-service.yaml
kubectl apply -f wp-deployment.yaml -f wp-service.yaml
kubectl get pods -o wide -n up-k8s-example-four
kubectl get svc -o wide -n up-k8s-example-four

siege -c 255 http://
kubectl top pods -n up-k8s-example-four
kubectl top nodes -n up-k8s-example-four

kubectl apply -f wp-autoscaler.yaml
kubectl get hpa -n up-k8s-example-four
kubectl get pods -o wide -n up-k8s-example-four
```

# Wordpress - helm
```bash
cd wordpress-helm

kubectl create serviceaccount --namespace kube-system tiller
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
helm init --service-account tiller --upgrade

helm upgrade --install --namespace up-k8s-example-five -f qa.values.yaml qa .
helm upgrade --install --namespace up-k8s-example-five -f staging.values.yaml staging .
helm upgrade --install --namespace up-k8s-example-five -f prod.values.yaml prod .
kubectl get pods -o wide -n up-k8s-example-five

helm delete --purge qa
helm delete --purge staging
helm delete --purge prod
```
