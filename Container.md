# About Containers And Kubernetes Container Cluster

## Statefulset on local K8s

## Helm

### Helm on Desktop

```sh
source <(helm completion bash)
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

```sh
helm init
helm repo update
helm install packagename
helm ls
helm delte packagename
helm rollback
```

Installing middleware

```sh
helm install --name mongok8s --set mongodbRootPassword=secretpassword,mongodbUsername=mongouser,mongodbPassword=mongoPa$$P0rd,mongodbDatabase=ks8 stable/mongodb
```

### Helm with AKS

https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm

```sh
kubectl apply -f ./Kubernetes/helm-rbac.yaml
helm init --service-account tiller --node-selectors '"beta.kubernetes.io/os"="linux"'
```

### Helm repo on ACR

```sh
az acr login -n azfunc
az acr helm repo add -n azfunc
```

### Configure ACR as Helm Repo

https://docs.microsoft.com/en-us/azure/container-registry/container-registry-helm-repos

## Istio and Service Mesh

https://istio.io/

### Istio on AKS

https://docs.microsoft.com/en-us/azure/aks/istio-install

### Intallation

https://istio.io/docs/setup/kubernetes/install/helm/

```sh
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.2.5 sh -
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
kubectl get crds | grep 'istio.io\|certmanager.k8s.io' | wc -l #23
helm install install/kubernetes/helm/istio --name istio --namespace istio-system
kubectl get svc -n istio-system
kubectl get svc -A
```

## Speanaker

https://www.spinnaker.io/

## Azure DevOps for Container CI/CD

## Managed Service Identity on Azure

- [AAD Pod Identity](https://github.com/Azure/aad-pod-identity)

## Tips

- Delete containers and images[Article](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)
- Set/Switch default namespace

```sh
kubectl.exe config set-context --current --namespace=default
```

## Network between containers on Dokcer

```sh
docker network create app-tier --driver bridge
docker run -it --rm --network app-tier bitnami/mongodb:latest mongo --host mongodb-server
```

## Local Storage on Kubernetes

https://k-miyake.github.io/blog/docker-k8s-mysql/

## Local Dashboard Configuration

```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml 
kubectl -n kube-system edit service kubernetes-dashboard
```

```diff:kubernetes-dashboard.yaml
...
spec:
  clusterIP: 10.99.127.230
  externalTrafficPolicy: Cluster
  ports:
+  - nodePort: 32744
-   - port: 443
+   port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
+  type: NodePort
...
```

Install cert for dashboard(https://github.com/kubernetes/dashboard/issues/2954)
Install OpenSSL if you don't have it.

```sh
mkdir .certs
openssl req -nodes -newkey rsa:2048 -keyout ./.certs/dashboard.key -out ./.certs/dashboard.csr -subj "/C=/ST=/L=/O=/OU=/CN=kubernetes-dashboard"
openssl x509 -req -sha256 -days 365 -in ./.certs/dashboard.csr -signkey ./.certs/dashboard.key -out ./.certs/dashboard.crt
kubectl create secret generic kubernetes-dashboard-certs --from-file=certs -n kube-system
kubectl create -f kubernetes-dashboard.yaml
```

## Command Tips

### Tail Command logs

```sh
kubectl logs podname -n ns --tail 1 -f
```

### Delete all resources in specific namespace

```sh
kubectl delete all --all -n {namespace}
```