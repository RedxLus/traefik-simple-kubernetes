# traefik-simple-kubernetes
Simple Ingress Traefik (no need HELM) with dashboard and easy configmap (traefik.toml)

## How to deploy?
This Treafik is made as an Ingress for deploy in Cloud. I tested and work in GCP.   

This Traefik come with this files inside the v1.7 folder:
* <b>traefik-Permisos.yaml</b> - Inside this YAML there are the permissions needed to Traefik.
* <b>traefik-Deployment.yaml</b> - Inside this YAML there are container of Traefik. You can change the image: traefik:1.7.20 to a new version if you want. The SSL and CONFIG are stored in a Secret.
* <b>traefik-Secret.yaml</b> - Inside this YAML there are the certificate SSL. Is optional, if you use a CDN like CloudFlare you don't need a real certificate and don't need to change any. If you want your own certificate just change the tls.crt and tls.key.
* <b>traefik-configmap.yaml</b> - Inside this YAML there are all the configuration of Traefik. You can easy modify the traefik.toml. [Here have all the possible configuration](https://github.com/helm/charts/tree/master/stable/traefik#configuration )
* <b>traefik-Service.yaml</b> - Inside this YAML there are Traefik service. Is needed to work. You can change to NodePort but as a Ingress is set to LoadBalancer. You need the Public IP of this service.
* <b>traefik-Service-dashboard.yaml</b> - Inside this YAML there are Traefik Dashboard Service. Is not need to Treafik works, you can delete if you don't use the dashboard. It is a ClusterIP service to connect with the Ingress and can access by the public IP of the Traefik.
* <b>traefik-Ingress-dashboard.yaml</b> - Inside this YAML there are Ingress file for Traefik Dashboard. Is not need to Treafik works, you can delete if you don't use the dashboard. You MUST edit it with your host.
  
  
First you need to clone this repository:  
`git clone https://github.com/RedxLus/traefik-simple-kubernetes.git`

Now you MUST change your domain in the <b>traefik-Ingress-dashboard.yaml</b> and CAN made some changes in the configuration <b>traefik-configmap.yaml</b>.
  
When you have everything ready now you have to apply in your Kuberentes Cluster. The files are going to deploy in the Kube-system namespace.  
`kubectl apply -f traefik-simple-kubernetes/*`

And no more. Just check the Public IP of the LoadBalancer of Traefik and modify your domain or subdomain with it to access.  
`kubectl get service release-name-traefik -n kube-system -w`
