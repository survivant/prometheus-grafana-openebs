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


