<!-- .slide: class="center dark" -->
<!-- .slide: data-background="../img/background/hands-on.jpg" -->
# Securing Kubernetes Clusters

<div class="label">Hands-on Time</div>


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## WARNING

The next few slides do **NOT** work on **EKS** and **GKE**.

For **EKS**, please follow the instructions from [Managing Users or IAM Roles for your Cluster
](https://docs.aws.amazon.com/eks/latest/userguide/add-user-role.html). Create a user and a cluster named *jdoe*. Once finished, continue from the **Deploying go-demo-2** slide.

For **GKE**, please follow the instructions from [Cloud Identity and Access Management Overview](https://cloud.google.com/iam/docs/overview) and [Kubernetes Engine Creating Cloud IAM Policies](https://cloud.google.com/kubernetes-engine/docs/how-to/iam#primitive). Create a user and a cluster named *jdoe*. Once finished, continue from the **Deploying go-demo-2** slide.


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Accessing Kubernetes API

```bash
CLUSTER_NAME=minikube

kubectl config view \
    -o jsonpath="{.clusters[?(@.name=='$CLUSTER_NAME')].cluster.server}"

kubectl config view \
    -o jsonpath="{.clusters[?(@.name=='$CLUSTER_NAME')].cluster.certificate-authority}"
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Accessing Kubernetes API

* We created `CLUSTER_NAME` variable
* We retrieved current context's server and certificate


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Creating Users

```bash
openssl version

mkdir -p cluster/keys

openssl genrsa -out cluster/keys/jdoe.key 2048

openssl req -new -key cluster/keys/jdoe.key \
    -out cluster/keys/jdoe.csr -subj "/CN=jdoe/O=devs"

ls -1 ~/.minikube/ca.*
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Creating Users

```bash
openssl x509 -req -in cluster/keys/jdoe.csr -CA ~/.minikube/ca.crt \
    -CAkey ~/.minikube/ca.key -CAcreateserial \
    -out cluster/keys/jdoe.crt -days 365

cp ~/.minikube/ca.crt cluster/keys/ca.crt

ls -1 cluster/keys
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Creating Users

```bash
SERVER=$(kubectl config view \
    -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.server}')

echo $SERVER

kubectl config set-cluster jdoe \
    --certificate-authority cluster/keys/ca.crt --server $SERVER

kubectl config set-credentials jdoe \
    --client-certificate cluster/keys/jdoe.crt \
    --client-key cluster/keys/jdoe.key
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Deploying go-demo-2

```bash
kubectl create -f auth/go-demo-2.yml --record --save-config
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Creating Users

```bash
kubectl config set-context jdoe --cluster jdoe --user jdoe

kubectl config use-context jdoe

kubectl config view

kubectl get pods

kubectl get all
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Pre-Defined Cluster Roles

```bash
kubectl config use-context minikube

kubectl get all

kubectl auth can-i get pods --as jdoe

kubectl get roles

kubectl get clusterroles
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Pre-Defined Cluster Roles

```bash
kubectl describe clusterrole view

kubectl describe clusterrole edit

kubectl describe clusterrole admin

kubectl describe clusterrole cluster-admin

kubectl auth can-i "*" "*"
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl create rolebinding jdoe --clusterrole view --user jdoe \
    -n default --save-config

kubectl get rolebindings

kubectl describe rolebinding jdoe

kubectl -n kube-system describe rolebinding jdoe
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl auth can-i get pods --as jdoe

kubectl auth can-i get pods --as jdoe --all-namespaces

kubectl delete rolebinding jdoe

cat auth/crb-view.yml

kubectl create -f auth/crb-view.yml --record --save-config

kubectl describe clusterrolebinding view
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl auth can-i get pods --as jdoe --all-namespaces

cat auth/rb-dev.yml

kubectl create -f auth/rb-dev.yml --record --save-config

kubectl -n dev auth can-i create deployments --as jdoe

kubectl -n dev auth can-i delete deployments --as jdoe
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl -n dev auth can-i "*" "*" --as jdoe

cat auth/rb-jdoe.yml

kubectl create -f auth/rb-jdoe.yml --record --save-config

kubectl -n jdoe auth can-i "*" "*" --as jdoe

kubectl describe clusterrole admin

cat auth/crb-release-manager.yml
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl create -f auth/crb-release-manager.yml --record --save-config

kubectl describe clusterrole release-manager

kubectl -n default auth can-i "*" pods --as jdoe

kubectl -n default auth can-i create deployments --as jdoe

kubectl -n default auth can-i delete deployments --as jdoe

kubectl config use-context jdoe
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl -n default run db --image mongo:3.3

kubectl -n default delete deployment db

kubectl config set-context jdoe --cluster jdoe --user jdoe \
    --namespace jdoe

kubectl config use-context jdoe

kubectl run db --image mongo:3.3
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Role And Cluster Role Bindings

```bash
kubectl delete deployment db

kubectl create rolebinding mgandhi --clusterrole=view \
    --user=mgandhi -n=jdoe
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Replacing Users With Groups

```bash
openssl req -in cluster/keys/jdoe.csr -noout -subject

cat auth/groups.yml

kubectl config use-context minikube

kubectl apply -f auth/groups.yml --record

kubectl -n dev auth can-i create deployments --as jdoe

kubectl config use-context jdoe
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Replacing Users With Groups

```bash
kubectl -n dev run new-db --image mongo:3.3
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## What Now?

```bash
# If minikube
kubectl config use-context minikube

# If EKS
kubectl config use-context \
    iam-root-account@devops24.$AWS_DEFAULT_REGION.eksctl.io

kubectl delete deploy db

kubectl delete rolebinding release-manager

kubectl delete ns jdoe dev
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## What Now?

```bash
kubectl delete -f auth/go-demo-2.yml
```