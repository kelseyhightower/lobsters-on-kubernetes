# Deploy MySQL


## Create Google Persistent Disk

```
$ gcloud compute disks create mysql

NAME        ZONE        SIZE_GB  TYPE         STATUS
mysql       us-west1-b  500      pd-standard  READY
```

## Create mysql PersistentVolume

```
$ kubectl create -f lobsters-on-kubernetes/pv/mysql.yaml

=>  persistentvolume "mysql" created
```

## Create mysql PersistentVolumeClaim

```
$ kubectl create -f lobsters-on-kubernetes/pvc/mysql.yaml 

=> persistentvolumeclaim "mysql" created
```

## Create mysql Secrets

```
$ kubectl create secret generic lobsters \
  --from-literal=root-password=l0bst3rs \
  --from-literal=mysql-password=lobsters \
  --from-literal='database-url=mysql2://lobsters:lobsters@mysql:3306/lobsters'

=> secret "lobsters" created
```

## Create mysql Deployment

```
$ kubectl create -f lobsters-on-kubernetes/deployments/mysql.yaml 

=> deployment "mysql" created
```

## Create mysql Service

```
$ kubectl create -f lobsters-on-kubernetes/services/mysql.yaml

=> service "mysql" created
```
