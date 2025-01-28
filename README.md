# Setting Up Minikube with PostgreSQL and Web Application Using Helm

This guide provides instructions for setting up a Kubernetes cluster using Minikube, deploying PostgreSQL with persistent storage, and workout management web application using Helm which is a package manager for Kubernetes.

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
- `postgres-statefulset.yaml`
- `persistent-volume-claims.yaml`
- `persistent-volumes.yaml`
- `postgres-configmap.yaml`
- `service.yaml`
  
In addition to the above yaml files the repository includes the _helpers.tpl and NOTES.txt files.

### Edit your `postgres-secret.yaml`
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ include "workout-app.fullname" . }}-postgres-secret
type: opaque
data:
  POSTGRES_PASSWORD: <base64-encoded-password>
```

### Edit your `web-app-secret.yaml`
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ include "workout-app.fullname" . }}-web-app-secret
type: opaque
data:
  DATABASE_URL: <base64-encoded-database-url>
```

DATABASE_URL should be in the following format:

```yaml
postgresql://<username>:<password>@workout-app-postgres-db:5432/workout
```
You need to update your DATABASE_URL to align with the dynamically created PostgreSQL service name. The service name is generated using the Helm template {{ include "workout-app.fullname" . }}-web-app-secret.

In this instance, the service name resolves to workout-app-postgres-db. 

#### Generating Base64 Encoded Credentials
Generate credentials using:
```bash
echo -n "<value>" | base64
```
Replace `<value>` with your desired credential.

---
You can also run the encode-secrets.sh script in the scripts/ directory, for encoding secrets.

Encode Secrets: If you're using the encode-secrets.sh script to encode secrets for use in Kubernetes secrets, run it like this:

```bash
./scripts/encode-secrets.sh <your-secret-value>
```

This will output the base64-encoded secret that you can use in your postgres-secret.yaml.



