
#------------------------------------------------------------------------------
# Start All Services on master and on tone node
#------------------------------------------------------------------------------
# https://kubernetes.io/docs/getting-started-guides/fedora/fedora_manual_config/
#
for SERVICES in etcd kube-apiserver kube-controller-manager kube-scheduler; do
    systemctl restart $SERVICES
    systemctl enable $SERVICES
    systemctl status $SERVICES
done
# Start the services on the node (fed-node)
for SERVICES in kube-proxy kubelet docker; do
    systemctl restart $SERVICES
    systemctl enable $SERVICES
    systemctl status $SERVICES
done

#------------------------------------------------------------------------------
#  Step by step
#------------------------------------------------------------------------------
# Docker Image 2.0.0
git clone https://github.com/kelseyhightower/lobsters.git
cp Dockerfile lobsters
docker build -t kelseyhightower/lobsters:2.0.0 lobsters
docker push kelseyhightower/lobsters:2.0.0
# Docker Image 2.0.1
cp custom.css lobsters/app/assets/stylesheets/local
docker build -t kelseyhightower/lobsters:2.0.1 lobsters
docker push kelseyhightower/lobsters:2.0.1

docker images

#Create Google Persistent Disk
gcloud compute disks create mysql
   #NAME        ZONE        SIZE_GB  TYPE         STATUS
   #mysql       us-west1-b  500      pd-standard  READY
#Create mysql PersistentVolume
kubectl create -f pv/mysql.yaml
   # persistentvolume "mysql" created
# Create mysql PersistentVolumeClaim
kubectl create -f pvc/mysql.yaml
   # persistentvolumeclaim "mysql" created
# Create mysql Secrets
kubectl create secret generic lobsters \
  --from-literal=root-password=l0bst3rs \
  --from-literal=mysql-password=lobsters \
  --from-literal='database-url=mysql2://lobsters:lobsters@mysql:3306/lobsters'
#secret "lobsters" created

# Secrets and ConfigMaps
kubectl create secret generic nginx --from-file nginx.conf

# MySQL
kubectl create -f deployments/mysql.yaml
kubectl get pods
cat services/mysql.yaml
kubectl create -f services/mysql.yaml
kubectl get pods
kubectl get svc
kubectl get pods                # at this point mysql is running on ip

# Web App
# Create Lobsters service
kubectl create -f services/lobsters.yaml
# Create Lobsters Deployment
cat deployments/lobsters.yaml
kubectl get secrets             # lobsters with type Opague -> with all secrets
kubectl create -f deployments/lobsters.yaml
kubectl get pods
kubectl get srv
cat jobs/lobsters-db-schema-load.yaml
kubectl create -f jobs/lobsters-db-schema-load.yaml
watch kubectl get jobs
# browser our app is running
cat jobs/lobsters-db-seed.yaml
kubectl create -f jobs/lobsters-db-seed.yaml
watch kubectl get jobs
# now able to login to web

# Make Replica
kubectl get pods
kubectl get svc
kubectl describe svc lobsters
kubectl get pods
kubectl logs <lobsters-<id>> -f

# Create a new Container, update it, marketing, change color
vim deployments/lobsters.yaml
kubectl apply -f deployments/lobsters.yaml              # send to cluster
watch kubectl get pods

# Rolling update
#kubectl set image deployment lobsters lobsters=kelseyhightower/lobsters:1.1.0

# How to do HTTPS, let's encrypt  --> custome extensios
vim extensions/certificate.yaml
kubectl create -f extensions/certificate.yaml
# kubernetes create REST endpoind, storege in it's backend a manage it
cat deployments/lobsters-nginx.yaml
   # configmap: name: nginx
cat configs/lobsters.conf
   # nginx.conf file
kubectl create configmap nginx --from-file configs/lobsters.yaml
   # now we have this config map in the system
kubectl get configmaps

# Create Secrets
cat deployments/kube-cert-manager.yaml
kubectl create -f deployments/kube-cert-manager.yaml
kubectl get pods
vim certificates/lobsters.yaml
kubectl get pods
# make sure there is no issue
kubectl describe pods <kube-cert-manager-<id>>
# at this point it was scheduled > now is creating volume ...
# submit the job
kubectl create -f certificates/lobsters.yaml
kubectl get pods
kubectl logs <kube-cert-manager-<id>> -c kube-cert-manager -f
  # -c --> multiple containters in the pod
  # live demo with DNS if i receive
  #   _acme-challenge.labsters.mydomain.com. DNS propagation complete
  #   lobsters.mydomain.com secret missing
  #   lobsters.mydomain.com secret created
  #   Watching for certificate events
  #   Starting reconciliation loop.  Ouu Snaap!
  #      at this poin we have the same interface for requesting certificate
  #      that we have for everything else in kubernetes
  #      -> if working we say
kubectl get secrets
  # we should see one for lobsters.mydomain.com
kubectl delete secrets lobsters.mydomain.com
  # it's declarated system we don't delete this certificate object we only
  #   deleting the secrets on this or inside the kubernetes
watch kubectl get secrets
  # in the log we can see new secrets, change is done online
vim deployments/lobsters-secure.yaml
kubectl apply -f deployments/lobsters-secure.yaml
kubectl get pods
  # doing now rolling update because the definition changed
  # if ok just ceck if still valid
kubectl get svc

#------------------------------------------------------------------------------
# CMDS
#------------------------------------------------------------------------------
kubectl create secret generic nginx --from-file nginx.conf
kubectl create -f deploy


