# Lobsters Demo

## Setup Mysql

```
gcloud compute disks create mysql
```

```
NAME        ZONE        SIZE_GB  TYPE         STATUS
mysql       us-west1-b  500      pd-standard  READY
```

```
kubectl create -f pv/mysql.yaml
```
```
persistentvolume "mysql" created
```

```
kubectl create -f pvc/mysql.yaml 
```
```
persistentvolumeclaim "mysql" created
```

```
kubectl create secret generic lobsters \
  --from-literal=root-password=l0bst3rs \
  --from-literal=mysql-password=lobsters \
  --from-literal='database-url=mysql2://lobsters:lobsters@mysql:3306/lobsters'
```

```
secret "lobsters" created
```

```
kubectl create -f deployments/mysql.yaml 
```
```
deployment "mysql" created
```

```
kubectl create -f services/mysql.yaml
```

```
service "mysql" created
```

## Deploy Lobsters

```
kubectl create -f deployments/lobsters.yaml
```

```
kubectl create -f services/lobsters.yaml
```

```
gcloud compute firewall-rules create lobsters --allow tcp:3000
```

### Database Setup Jobs

```
kubectl create -f jobs/lobsters-db-schema-load.yaml
```

```
kubectl create -f jobs/lobsters-db-seed.yaml
```
