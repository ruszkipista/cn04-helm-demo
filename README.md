# cn04-helm-demo
Continuous Delivery demo using Helm templating on ArgoCD - Cloud Native Fundamentals course @Udacity

1. install Vagrant
2. clone this repo as your project folder
3. from the project folder issue the following commands:
```
# start SUSE vagrant box
vagrant up
# log into SUSE vagrant box
vagrant ssh
# enter into administrator mode
sudo su
# install missing piece of sofware onto the SUSE vagrant box
zypper install -t pattern apparmor
# install K3s
curl -sfL https://get.k3s.io | sh -
# create namspace for ArgoCD
kubectl create namespace argocd
# install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# wait until all pods are ready (8 mins), check status with
kubectl get pods -n argocd
# create new service to expose argocd-server externally (allow access to webgui)
kubectl apply -f /vagrant/deploy/argocd/server-nodeport.yaml 
# extract password for user ‘admin’
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
# Login to ArgoCD webgui with username admin, password (from previous step) using https://192.168.50.4:30008
# ...
# launch a 3 pods NGINX server with static manifests
kubectl apply -f /vagrant/deploy/argocd/app-static.yaml
# in the ArgoCD webgui click on SYNC button
# ...
# launch a 1 pod NGINX (development) server with templated manifests
kubectl apply -f /vagrant/deploy/argocd/app-templated-default.yaml
# in the ArgoCD webgui click on SYNC button
# ...
# launch a 2 pods NGINX (staging) server with templated manifests
kubectl apply -f /vagrant/deploy/argocd/app-templated-staging.yaml
# in the ArgoCD webgui click on SYNC button
# ...
# launch a 3 pods NGINX (production) server with templated manifests
kubectl apply -f /vagrant/deploy/argocd/app-templated-prod.yaml
# in the ArgoCD webgui click on SYNC button
# ...