## Cert Manager

### Installing Cert-Manager

The first step is to add the Jetstack repository:

    helm repo add jetstack https://charts.jetstack.io
    helm repo update

Install Cert-Manager with CRDs into your cluster

    helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --set installCRDs=true


### Configure The Let's Encrypt Certificate Issuer

Create a YAML file named letsencrypt-production.yaml

    apiVersion: cert-manager.io/v1
    kind: ClusterIssuer
    metadata:
        name: letsencrypt-production
    spec:
      acme:
        server: https://acme-v02.api.letsencrypt.org/directory
        email: example@domain.com
        privateKeySecretRef:
          name: letsencrypt-production
        solvers:
          - http01:
              ingress:
                class: nginx



    kubectl create -f letsencrypt-issuer-staging.yaml


### Obtain an HTTPS Certificate


    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    name: wordpress
    annotations:
        kubernetes.io/ingress.class: nginx
        cert-manager.io/cluster-issuer: letsencrypt-production
    spec:
      rules:
        - http:
            paths:
            - path: /
              pathType: Prefix
              backend:
                service:
                    name: wordpress
                    port:
                    number: 80
    tls:
     - hosts:
       - example.com