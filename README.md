#In this tutorial I gather yaml file for implementing gitlab and Jenkins on kubernetes cluster.
Including 2 scenario , first one is deploy gitlab with exposing service using imprative command line and ingress resource and implementing ssl-tls on port 443 .
The second scenario is to implement Jenkins with service and ingress resource on http without ssl-tls on port 80 and nodeport for agent connectivity.

1-
For gitlab there are 2 files which is deploy gitlab as a deployment with persistent volume and persistent volume claim (gitlab-deployment.yaml) 
on the second file is how to implement gitlab Ingress resource based on port 443 SSL TLS. (gitlab-ingress-resource.yaml)
Before ran into the ingress resource for gitlab you have make ssl-tls file and then provide it to the secret component , you can use valid cert or using openssl or either using lets encrypt.
A. make dns pointer or subdomain as you need and deploy gitlab-deployment.yaml
B. make your self sign certificate or any other tools (I just used lets encrypt for domain )
c. make secret in type of tls and import your cert/key files in it

    1.1
    kubectl create secret tls tls-secret \
  --namespace=kubernetes-dashboard \
  --cert=/etc/letsencrypt/live/gitlab.yourdomain.com/fullchain.pem \
  --key=/etc/letsencrypt/live/gitlab.yourdomain.com/privkey.pem

    1.2 # make a service for your gitlab using imprative command:
          kubectl expose deployment -n gitlab-ns gitlab-deployment --name=gitlab-clusterip-svc --port=443 --target-port=443
          kubectl expose deployment -n gitlab-ns gitlab-deployment --name=gitlab-clusterip-svc-ssh --port=22 --target-port=22
          
Now it is a time to deploy your ingress resource yaml file ==> (gitlab-ingress-resource.yaml)
  
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
2-
For Jenkins there are 3 files including Jenkins deployment with persistent volume and persistent volume claim .
and the Ingress resource and also Nodeport connectivity for Jenkins agent.
A. deploy Jenkins using Jenkins-deployment.yaml on your cluster , !!!!!!! this time it has service you do not need to use the imprative command to expose it. !!!!!!!!
B. deploy ingress resource using Jenkins-ingress-resource.yaml .
C. deploy Jenkins-nodeport-for-agent-connectivity.yaml , for other agents which they want to connect to your master and be a worker.
