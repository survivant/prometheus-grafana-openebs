Need to create the namespace metrics

kubectl create ns metrics


#Install 
helm install monitor -n metrics .

# Delete
helm delete monitor -n metrics

voici les liens :

#Prometheus
http://10.1.7.114:30004/
#AlertManager
http://10.1.7.114:30005/#/alerts
#Grafana
http://10.1.7.114:30006/

#
helm install ingress-nginx ingress-nginx/ingress-nginx --set controller.service.externalIPs[0]=10.1.7.114 --set controller.service.type=NodePort



Need to install if the storageclass = local-path is used
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml

or with OpenEBS Local PV.

https://docs.openebs.io/docs/next/uglocalpv-hostpath.html


##Setup ingress-controller

if you want to use **Ingress** you need to install a ingress-controller.  There are few ingress-controller availables https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/, but we choose this one
NGINX Ingress Controller

https://kubernetes.github.io/ingress-nginx/deploy/#using-helm

    helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
    helm repo update


### use Ingress-controller in LoadBalancer mode (with LoadBalancer enabled like metallb)
    helm install ingress-nginx ingress-nginx/ingress-nginx --set controller.service.externalIPs[0]=10.1.7.114 --set controller.service.type=LoadBalancer

### use Ingress-controller in NodePort mode (when you don't have LoadBalancer)
    helm install ingress-nginx ingress-nginx/ingress-nginx --set controller.service.externalIPs[0]=10.1.7.114 --set controller.service.type=NodePort

## Install this chart

### using ClusterIP with Ingress (default)
    helm --kube-context cluster114 install monitor -n metrics .

####Grafana
http://10.1.7.114/grafana


### using NodePort instead of ClusterIP
    helm --kube-context cluster114 install monitor -n metrics .  --set grafana.service.type=NodePort --set grafana.service.nodePort=30006 --set prometheus.alertmanager.service.type=NodePort --set prometheus.alertmanager.service.nodePort=30005 --set prometheus.server.service.type=NodePort --set prometheus.server.service.nodePort=30004

####Prometheus
http://10.1.7.114:30004/
####AlertManager
http://10.1.7.114:30005/
####Grafana
http://10.1.7.114:30006/

### disable the persistence
    helm --kube-context cluster114 install monitor -n metrics . --set grafana.persistence.enabled=false --set prometheus.alertmanager.persistentVolume.enabled=false --set prometheus.server.persistentVolume.enabled=false


## test alertmanager (standalone) NOT READY.. There are PR that need to be merge first
helm --kube-context cluster114 delete monitor -n metrics
helm --kube-context cluster114 install monitor -n metrics . --set alertmanager.enabled=true --set prometheus.alertmanager.enabled=false --set grafana.persistence.enabled=false --set prometheus.alertmanager.persistentVolume.enabled=false --set prometheus.server.persistentVolume.enabled=false


