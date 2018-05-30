
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
# MySQL
kubectl create -f deployments/mysql.yaml
kubectl get pods
cat services/mysql.yaml
kubectl create -f services/mysql.yaml
kubectl get pods
kubectl get svc
kubectl get pods                # at this point mysql is running on ip

# Web App
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
# login to web


#------------------------------------------------------------------------------
# CMDS
#------------------------------------------------------------------------------
kubectl create secret generic nginx --from-file nginx.conf
kubectl create -f deploy


