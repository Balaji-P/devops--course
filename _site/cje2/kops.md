## Hands-On Time

---

# Creating A Production-Ready Kubernetes Cluster


## Prerequisites

* [Git](https://git-scm.com/downloads)
* [AWS account](https://aws.amazon.com)
* [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)
* [jq](https://stedolan.github.io/jq/)
* [kops](https://github.com/kubernetes/kops#installing) or [Docker for Windows](https://www.docker.com/docker-windows)
* GitBash (if Windows)


## Creating AWS keys

---

```bash
open "https://console.aws.amazon.com/iam/home#/security_credential"

export AWS_ACCESS_KEY_ID=[...]

export AWS_SECRET_ACCESS_KEY=[...]

export AWS_DEFAULT_REGION=us-east-2
```


## IAM

---

```bash
aws iam create-group --group-name kops

aws iam attach-group-policy --group-name kops \
    --policy-arn arn:aws:iam::aws:policy/AmazonEC2FullAccess

aws iam attach-group-policy --group-name kops \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess

aws iam attach-group-policy --group-name kops \
    --policy-arn arn:aws:iam::aws:policy/AmazonVPCFullAccess

aws iam attach-group-policy --group-name kops \
    --policy-arn arn:aws:iam::aws:policy/IAMFullAccess

aws iam create-user --user-name kops
```


## IAM

---

```bash
aws iam add-user-to-group --user-name kops --group-name kops

aws iam create-access-key --user-name kops >kops-creds

cat kops-creds

export AWS_ACCESS_KEY_ID=$(cat kops-creds | \
    jq -r '.AccessKey.AccessKeyId')

export AWS_SECRET_ACCESS_KEY=$(cat kops-creds | \
    jq -r '.AccessKey.SecretAccessKey')
```


## Choosing Availability Zones

---

```bash
aws ec2 describe-availability-zones --region $AWS_DEFAULT_REGION

# If Windows, use `tr '\r\n' ', '` instead of `tr '\n' ','`
export ZONES=$(aws ec2 describe-availability-zones \
    --region $AWS_DEFAULT_REGION | jq -r \
    '.AvailabilityZones[].ZoneName' | tr '\n' ',' | tr -d ' ')

ZONES=${ZONES%?}

echo $ZONES
```


## Generating SSH Keys

---

```bash
mkdir -p cluster

cd cluster

aws ec2 create-key-pair --key-name devops23 \
    | jq -r '.KeyMaterial' >devops23.pem

chmod 400 devops23.pem

ssh-keygen -y -f devops23.pem >devops23.pub
```


## Creating Cluster State Storage

---

```bash
export NAME=devops23.k8s.local

export BUCKET_NAME=devops23-$(date +%s)

aws s3api create-bucket --bucket $BUCKET_NAME \
    --create-bucket-configuration \
    LocationConstraint=$AWS_DEFAULT_REGION

export KOPS_STATE_STORE=s3://$BUCKET_NAME
```


## kops In Windows

---

```bash
mkdir config

# Windows only
alias kops="docker run -it --rm \
    -v $PWD/devops23.pub:/devops23.pub \
    -v $PWD/config:/config \
    -e KUBECONFIG=/config/kubecfg.yaml \
    -e NAME=$NAME -e ZONES=$ZONES \
    -e AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
    -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
    -e KOPS_STATE_STORE=$KOPS_STATE_STORE \
    vfarcic/kops"
```


## Creating A Cluster

---

```bash
kops create cluster --name $NAME --master-count 3 --node-count 3 \
    --master-size t2.small --node-size t2.xlarge --zones $ZONES \
    --master-zones $ZONES --ssh-public-key devops23.pub \
    --networking kubenet --yes

# Windows only
kops export kubecfg --name ${NAME}

# Windows only
export KUBECONFIG=$PWD/config/kubecfg.yaml

kubectl cluster-info

kops validate cluster
```


<!-- .slide: data-background="../docs/img/kops-infra-servers.png" data-background-size="contain" -->


<!-- .slide: data-background="../docs/img/kops-infra-nodeup.png" data-background-size="contain" -->


## Exploring System Pods

---

```bash
kubectl -n kube-system get pods
```


<!-- .slide: data-background="../docs/img/kops-infra-components.png" data-background-size="contain" -->


## Accessing The Cluster API

---

```bash
aws elb describe-load-balancers

kubectl config view
```


<!-- .slide: data-background="../docs/img/kops-elb-api.png" data-background-size="contain" -->


## Accessing Cluster Nodes

---

```bash
kubectl create -f \
    https://raw.githubusercontent.com/kubernetes/kops/master/addons/ingress-nginx/v1.6.0.yaml

kubectl -n kube-ingress get all

aws elb describe-load-balancers

CLUSTER_DNS=$(aws elb describe-load-balancers | jq -r \
    ".LoadBalancerDescriptions[] | select(.DNSName \
    | contains (\"api-devops23\") | not).DNSName")
```
