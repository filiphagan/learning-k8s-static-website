# Static website on Kubernetes

## Description
Learning task: Static website published on local Kubernetes cluster. 
Service is exposed through Ingress at `my.dummy.domain`
Content of the website is located in `./contents/index.html`

Requirements: kubernetes, kubectl, docker

Files:
- kustomization.yml : file to generate custom ConfigMap
- website_deployment.yml : Kubernetes deployment definition
- contents/index.html : static website source code
- README.md


## Execution

1) Run local Kubernetes cluster. I'm running Kubernetes on [Docker Desktop](https://www.docker.com/products/docker-desktop/)


2) Set a custom domain name on localhost, for example `my.dummy.domain`: 
   - [Linux] add line `127.0.0.1 my.dummy.domain` in `/etc/hosts` 
   - [Windows] add the same line in `/Windows/System32/drivers/etc/hosts`
   

3) Create NGINX ingress controller

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.2/deploy/static/provider/cloud/deploy.yaml

# Optional: check pod state
kubectl get pods --namespace=ingress-nginx
```

4) Create the deployment

```
kubectl apply -k ./

# Optional: Preview of generated ConfigMap
kubectl kustomize ./
```

5) Port forwarding. After running this command our service will be available at http://my.dummy.domain:8080

```
kubectl port-forward --namespace=ingress-nginx service/ingress-nginx-controller 8080:80
```




