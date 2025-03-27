# Kubernetes-ArgoCD-Rancher
This project connects your local kubernetes (Rancher desktop) to your GIT repo using ArgoCD.
ArgoCD will pull your config files (Deployment, Services, etc) from the GIT and any changes made there will be reflected in your cluster.

Prerequisites 

If you can deploy an nginx cluster locally...then you've got what's needed!

Commands:

SETTING UP ARGOCD

alias k=kubectl # why write kubectl when you can just type k!

k create ns argocd #create a namespace

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml #install argo

k get pods -n argocd #check for pods

kubectl get svc -n argocd #check for services

kubectl port-forward service/argocd-server 8080:80 -n argocd # access the argo gui server locally on port 8080

k get secrets -n argocd #login details are stored here

k get secret argocd-initial-admin-secret -o yaml -n argocd #display your secret in a yaml format

echo foo|base64 -d  #replace foo with the password that was displayed in yaml


SETTING UP GIT LOCALLY (These are the steps I followed...My github repo (named lokiargo) had nothing...you can make your new empty repo and follow these steps... tweak and replace my reponame and link with yours)

#terminal

mkdir gitty 

cd gitty

git clone https://github.com/LokiSahetia/lokiargo.git

cd lokiargo   

mkdir lokinginx

cd lokinginx

nano nginxds.yaml #nginx deployment and service with a nodeport

Copy the contents from my repo


cd ..

git add lokinginx  

git commit -m "first commit" 

git push origin main

add username and password…Use pat token for password…pat token is created using

https://github.com/settings/tokens



nano application.yaml #argo configuration

Copy the contents

kubectl create namespace myapp #this is the namespace where our nginx deploynment and services will be deployed

git add .

git commit -m "application file uploaded"

git push origin main 

kubectl apply -f application.yaml #with this, your kubernetes will connect to github and keep a track of your configuration files...any 

changes committed will be reflected in the cluster.
check it on argo cd page 
also, change and check the sync
