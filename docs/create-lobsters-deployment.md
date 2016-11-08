# Create Lobsters Deployment

## Create lobsters Service

```
kubectl create -f lobsters-on-kubernetes/services/lobsters.yaml
```

```
gcloud compute firewall-rules create lobsters --allow tcp:3000
```

## Create lobsters Deployment

```
kubectl create -f lobsters-on-kubernetes//deployments/lobsters.yaml
```
