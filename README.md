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

7. Apply the `systole-infrastructure-development.yaml` manifest:

   ```
   kubectl apply -f systole-infrastructure-development.yaml
   ```

   This will deploy the infrastructure components to the Kind cluster.

That's it! You now have a development environment set up with ArgoCD, Argo Workflows, and Bitnami Sealed-Secrets. You can modify the Kubernetes manifests to fit your needs and deploy them to your production environment.
