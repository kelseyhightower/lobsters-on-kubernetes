# Create Lobsters Deployment

## Create lobsters Service

```
kubectl create -f services/lobsters.yaml
```

```
gcloud compute firewall-rules create lobsters --allow tcp:3000
```

## Create lobsters Deployment

```
kubectl create -f deployments/lobsters.yaml
```
