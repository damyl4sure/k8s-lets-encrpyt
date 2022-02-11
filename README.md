#Generate Certificate and Create Secret
https://medium.com/avmconsulting-blog/how-to-secure-applications-on-kubernetes-ssl-tls-certificates-8f7f5751d788 

# Clone the repo and follow the steps

# Don't change the lets-encrypt naming convention for prod and staging 

# 1. Generate certificate for FQDN

openssl genrsa -out ca.key 2048

openssl req -x509 -new -nodes -days 365 -key ca.key -out ca.crt -subj "/CN=soro-lingo.com"


# 2. Create cluster issuer for staging and prod 

kubectl apply -f lets-encrypt-clusterissuer-staging.yaml

kubectl apply -f lets-encrypt-clusterissuer-prod.yaml

# 3. Create secrets 

kubectl -n soro-develop create secret tls soro-tls --key ca.key --cert ca.crt

kubectl -n soro-develop create secret tls letsencrypt-staging-tls --key ca.key --cert ca.crt

kubectl -n soro-develop create secret tls letsencrypt-prod-tls --key ca.key --cert ca.crt

# 4. Configure certificates for staging and prod

kubectl apply -f certificate-staging.yaml

kubectl apply -f certificate-prod.yaml

# 5. Added annotation to the ingress (Staging annotation first then swicth over to production)

cert-manager.io/cluster-issuer: letsencrypt-staging

cert-manager.io/cluster-issuer: letsencrypt-prod

# kubernetes letencrypt configuration

https://cert-manager.io/docs/tutorials/acme/ingress/

https://www.youtube.com/watch?v=etC5d0vpLZE



# for wildcard, DNS01  solver must be used as the issuer.
https://cert-manager.io/docs/configuration/acme/dns01/azuredns/ 


https://faun.pub/wildcard-k8s-4998173b16c8

https://medium.com/emvi/wildcard-ssl-certificates-on-kubernetes-using-acme-dns-fde583a69eb5


https://github.com/fbeltrao/aks-letsencrypt
