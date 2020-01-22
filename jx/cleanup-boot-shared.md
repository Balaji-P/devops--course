## Cleanup

```bash
cd ..

hub delete -y $GH_USER/environment-jx-rocks-staging

hub delete -y $GH_USER/environment-jx-rocks-production

hub delete -y $GH_USER/environment-jx-rocks-dev

hub delete -y $GH_USER/jx-go

hub delete -y $GH_USER/jx-serverless
```


## Cleanup

```bash
hub delete -y $GH_USER/jx-prow

hub delete -y $GH_USER/jx-knative
```


## Cleanup

```bash
rm -rf ~/.jx/environments/$GH_USER/environment-jx-rocks-*

rm -rf jx-go

rm -rf jx-serverless

rm -rf jx-prow

rm -rf jx-knative

rm -rf environment-jx-rocks-*
```

<!--
unset KUBECONFIG

gcloud container clusters delete jx-rocks --region us-east1 --quiet

gcloud compute disks delete --zone us-east1-b $(gcloud compute disks \
    list --filter="zone:us-east1-b AND -users:*" \
    --format="value(id)") --quiet
gcloud compute disks delete --zone us-east1-c $(gcloud compute disks \
    list --filter="zone:us-east1-c AND -users:*" \
    --format="value(id)") --quiet
gcloud compute disks delete --zone us-east1-d $(gcloud compute disks \
    list --filter="zone:us-east1-d AND -users:*" \
    --format="value(id)") --quiet
-->