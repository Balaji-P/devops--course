<!-- .slide: class="center dark" -->
<!-- .slide: data-background="../img/background/hands-on.jpg" -->
# Defining And Running Serverless Deployments

<div class="label">Hands-on Time</div>


<!-- .slide: data-background="img/knative-request.png" data-background-size="contain" -->


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## New Serverless Application

```bash
cat charts/jx-knative/templates/ksvc.yaml

cat charts/jx-knative/values.yaml

kubectl --namespace jx-staging get pods
```


<!-- .slide: data-background="img/knative-scale-to-zero.png" data-background-size="contain" -->


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## New Serverless Application

```bash
kubectl run siege --image yokogawa/siege --generator "run-pod/v1" \
     -it --rm -- -c 300 -t 20S "$ADDR/" \
     && kubectl --namespace jx-staging get pods
```


<!-- .slide: data-background="img/knative-scale-to-three.png" data-background-size="contain" -->


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## New Serverless Application

```bash
kubectl --namespace jx-staging get pods
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Serverless With PRs

```bash
git checkout -b my-pr

cat main.go | sed -e "s@example@Knative@g" | tee main.go

git add . && git commit -m "Added something"

git push --set-upstream origin my-pr

jx create pullrequest --title "Serverless with Knative" \
    --body "What I can say?" --batch-mode
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Serverless With PRs

# It's a no-brainer


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Serverless With PRs

```bash
export BRANCH=[...] # e.g., `PR-109`

jx get activities --filter jx-knative/$BRANCH --watch

export GH_USER=[...]

PR_NAMESPACE=$(echo jx-$GH_USER-jx-knative-$BRANCH \
    | tr '[:upper:]' '[:lower:]')

kubectl --namespace $PR_NAMESPACE get pods
```


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Serverless With PRs

```bash
export PR_ADDR=$(kubectl --namespace $PR_NAMESPACE \
    get ksvc jx-knative --output jsonpath="{.status.url}")

curl "$PR_ADDR"

jx repo --batch-mode
```

* Merge the PR


<!-- .slide: class="dark" -->
<div class="eyebrow"></div>
<div class="label">Hands-on Time</div>

## Serverless With PRs

```bash
jx get activities --filter jx-knative/master --watch

jx get activities --filter environment-$CLUSTER_NAME-staging/master \
    --watch

curl "$ADDR/"

kubectl --namespace $PR_NAMESPACE get pods

kubectl --namespace jx-staging get pods

cd ..
```
