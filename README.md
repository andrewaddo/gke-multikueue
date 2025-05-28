# gke-multikueue

## Guide

The instructions are available at https://github.com/ACW101/ai-on-gke/tree/multikueue/tutorials-and-examples/workflow-orchestration/dws-multiclusters-example

## Set up

Get the example folder

```bash
mkdir -p dws-multiclusters-example && cd dws-multiclusters-example && git init && git remote add origin https://github.com/ACW101/ai-on-gke.git && git sparse-checkout set tutorials-and-examples/workflow-orchestration/dws-multiclusters-example && git pull origin multikueue
```

## Troubleshooting

1. Missing kubectl context for worker-asia-southeast1. Manually create it.

```bash
kubectl config get-contexts
gcloud container clusters get-credentials worker-asia-southeast1 --region asia-southeast1
kubectl config rename-context gke_addo-argolis-demo_asia-southeast1_worker-asia-southeast1 worker-asia-southeast1
kubectl config get-contexts
```

1. Auth issue: the pod could not generate auth token using the service account. Explicitly bind the SA with the KSA.

```bash
gcloud iam service-accounts add-iam-policy-binding \
    --role="roles/iam.workloadIdentityUser" \
    --member="principal://iam.googleapis.com/projects/827859227929/locations/global/workloadIdentityPools/addo-argolis-demo.svc.id.goog/subject/ns/default/sa/default" \
    --project="addo-argolis-demo" \
    827859227929-compute@developer.gserviceaccount.com
```

```bash
gcloud iam service-accounts add-iam-policy-binding 827859227929-compute@developer.gserviceaccount.com --member=serviceAccount:827859227929-compute@developer.gserviceaccount.com --role=roles/iam.serviceAccountTokenCreator
```
