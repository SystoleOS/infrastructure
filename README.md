# SystoleOS Infrastructure

This repository contains a set of Kubernetes manifests to provision ArgoCD, Argo Workflows, and Bitnami Sealed-Secrets.

## Setting up a development environment

To set up a development environment for this project using Kind, follow these steps:

1. Install [Docker](https://www.docker.com/get-started) and [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) on your machine.
2. Clone this repository to your local machine.
3. Open a terminal window and navigate to the root directory of the project.
4. Run the following command to create a Kind cluster:

   ```
   kind create cluster --name systole-infrastructure
   ```

5. Once the Kind cluster is up and running, create the `argocd` namespace:

   ```
   kubectl create namespace argocd
   ```

6. Install ArgoCD in the `argocd` namespace:

   ```
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
   
  at this point you may want to run `watch kubectl get all --all-namespaces` to check for the state of the resources and wait until all the pods are up an running. 
  

7. Apply the `systole-infrastructure-development.yaml` manifest:

   ```
   kubectl apply -f systole-infrastructure-development.yaml
   ```

   This will deploy the infrastructure components to the Kind cluster. Similarly to the previous point, you may want to run `watch kubectl get all --all-namespaces` to check for the state of the resources and wait until all the pods are up an running. 

8. ArgoCD runs a server that provides a web interface, which is exposed on port 80 within the cluster. To access it from your local machine, you need to set up a port-forward to the ArgoCD server service. Run the following command:

``` sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```


This command forwards requests from `http://localhost:8080` on your local machine to the ArgoCD server on port 443 in your Kind cluster.

9. Now, open a web browser on your local machine and navigate to `http://localhost:8080`. You should see the ArgoCD login page.

For ArgoCD version 1.8 and earlier, the initial password is the `argocd-server` pod name. Retrieve it with the following command:



```
kubectl -n argocd get pods -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2

```

For ArgoCD version 1.9 and later, the initial password is stored in the `argocd-initial-admin-secret` secret. Retrieve it with this command:

``` sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```


10. Use `admin` as the username and the output from the appropriate command above as the password to log in to the ArgoCD web interface.

**Note:** Remember, these steps are for a development environment. In a production environment, you would want to expose the ArgoCD server behind a secure ingress and set up a proper authentication method.

That's it! You now have a development environment set up with ArgoCD, Argo Workflows, and Bitnami Sealed-Secrets. You can modify the Kubernetes manifests to fit your needs and deploy them to your production environment.
