# my-k8s-app
kubernetes manifests for Argo CD demo
Absolutely! I can create a **ready-to-use `README.md`** for your **ArgoCD + Nginx app repo** (or any Kubernetes manifests repo). This README will explain what the repo is, how to deploy it with ArgoCD, and how to verify it.

Here’s an example:

---

```markdown
# ArgoCD Nginx Demo

This repository contains a simple **Nginx application** deployed to Kubernetes using **ArgoCD**. It demonstrates a basic GitOps workflow.

## Repository Structure

```

.
├── k8s/
│   ├── deployment.yaml   # Nginx Deployment
│   └── service.yaml      # Nginx Service
└── README.md

````

- `k8s/` – Contains Kubernetes manifests for the app.
- `deployment.yaml` – Creates a Deployment with Nginx container.
- `service.yaml` – Creates a ClusterIP Service to expose Nginx internally.

---

## Prerequisites

- Kubernetes cluster (e.g., Minikube, EKS, or any cluster)
- ArgoCD installed and running
- `kubectl` CLI configured to access your cluster
- `argocd` CLI (optional, if using CLI instead of UI)

---

## Deploy Using ArgoCD Web UI

1. Open ArgoCD UI (e.g., `https://localhost:8080`) and log in.
2. Click **“New App”**.
3. Fill the form:
   - **Application Name:** `my-app`
   - **Project:** `default`
   - **Repository URL:** `https://github.com/<your-username>/<repo-name>.git`
   - **Revision:** `HEAD` or `main`
   - **Path:** `k8s/`
   - **Cluster:** `https://kubernetes.default.svc`
   - **Namespace:** `default`
4. Click **Create**.
5. Click **Sync** to deploy the application.

---

## Verify Deployment

### Check pods

```bash
kubectl get pods -n default
````

You should see something like:

```
nginx-deployment-xxxx   1/1   Running   0   2m
```

### Check service

```bash
kubectl get svc -n default
```

You should see:

```
nginx-service   ClusterIP   10.96.xx.xx   <none>   80/TCP   2m
```

### Access Nginx

**Option 1: Port-forward**

```bash
kubectl port-forward svc/nginx-service 8080:80 -n default
```

Open in browser: `http://localhost:8080`

**Option 2: Minikube service**

```bash
minikube service nginx-service -n default
```

---

## Sync Updates

Whenever you update manifests in Git:

1. Open ArgoCD UI and click **Refresh**.
2. Click **Sync** to apply changes to the cluster.

> Optionally, enable **Auto-Sync** in ArgoCD to automatically deploy changes.

---

## Notes

* This is a **demo repository** for learning GitOps with ArgoCD.
* You can replace Nginx with your own applications by updating manifests in the `k8s/` folder.

```
