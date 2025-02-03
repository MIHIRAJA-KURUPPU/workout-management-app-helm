# Setting Up Minikube with PostgreSQL and Web Application Using Helm

This guide provides instructions for setting up a Kubernetes cluster using Minikube, deploying PostgreSQL with persistent storage, and workout management web application using Helm which is a package manager for Kubernetes.  This repository contains the Helm chart for deploying the `workout-app` in a Kubernetes environment. It includes the necessary templates for Kubernetes resources like Deployment, StatefulSet, Service, PVC, Secrets, and more.

---

## Prerequisites

- **Minikube**: Ensure Minikube is installed. Follow the [official documentation](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download) to set it up.
- **kubectl**: Configure an alias for convenience:
  ```bash
  alias kubectl="minikube kubectl --"
  ```

---

## Step 1: Start Minikube

Start your Minikube cluster:
```bash
minikube start
```

To access the Minikube VM, use:
```bash
minikube ssh
```
Switch to root inside the VM:
```bash
sudo su -
```

---

## Step 2: Kubernetes YAML Files

The repository includes the following YAML files as templates:
- `deployment.yaml`
- `statefulset.yaml`
- `persistent-volume-claims.yaml`
- `persistent-volumes.yaml`
- `configmap.yaml`
- `service.yaml`
- `secret.yaml`
  
In addition to the above yaml files the repository includes the _helpers.tpl and NOTES.txt files.


## Workout App Helm Chart

### 1. Create the Helm Chart

Create a Helm chart named `workout-app` in your project directory. 

- Replace the `templates/` directory with the required Kubernetes resource files, such as `deployment.yaml`, `statefulset.yaml`, `service.yaml`, `pvc.yaml`, `secrets.yaml`, and other necessary files.
- Update the `helpers.tpl` file to include functions that dynamically generate useful names.
- Modify `values.yaml` to define application-specific configurations like database details, image settings, and replica counts.

### 2. Deploy or Upgrade the Application

To install or upgrade the `workout-app` release, run the following command:

```bash
helm upgrade --install workout-management-app . \
  --namespace workout-app-ns \
  --create-namespace \
  --set appuser.username=<your-username> \
  --set appuser.userpassword=<your-password> \
  --set postgres.auth.password=<your-password>

```

The --create-namespace flag will ensure that the namespace workout-app-ns is created if it does not already exist.
This command will install the workout-app release or upgrade it if it's already installed.

