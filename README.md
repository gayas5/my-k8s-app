````markdown
# my-k8s-app

Kubernetes manifests for an **ArgoCD demo** using Nginx.

---

## Quick Start

Deploy the Nginx app in **3 simple steps**:

```bash
# 1. Apply Deployment
kubectl apply -f k8s/deployment.yaml

# 2. Apply Service
kubectl apply -f k8s/service.yaml

# 3. Verify Pods & Service
kubectl get pods -n default
kubectl get svc -n default
````

> Optionally, you can deploy using **ArgoCD** for GitOps-style management.

---

## Directory Structure

```
k8s/
├── deployment.yaml  # Nginx Deployment
├── service.yaml     # Nginx Service
└── README.md        # This file
```

---

## Prerequisites

* A running Kubernetes cluster (Minikube, EKS, or any cluster)
* ArgoCD installed and running
* `kubectl` CLI configured to access your cluster
* `argocd` CLI (optional, if using CLI instead of UI)

---

## Deployment

The `deployment.yaml` file creates a Deployment for Nginx with:

* A specified number of replicas
* A container running the latest Nginx image
* Labels for pod selection by the Service

### Apply Deployment

```bash
kubectl apply -f k8s/deployment.yaml
```

---

## Service

The `service.yaml` file exposes the Nginx Deployment using a Kubernetes Service. By default, it is a **ClusterIP** service (accessible only within the cluster). You can change the type to `NodePort` or `LoadBalancer` if needed.

### Apply Service

```bash
kubectl apply -f k8s/service.yaml
```

---

## Deploy Using ArgoCD Web UI

1. Open ArgoCD UI (e.g., `https://localhost:8080`) and log in.
2. Click **“New App”**.
3. Fill the form:

   * **Application Name:** `my-app`
   * **Project:** `default`
   * **Repository URL:** `https://github.com/<your-username>/<repo-name>.git`
   * **Revision:** `HEAD` or `main`
   * **Path:** `k8s/`
   * **Cluster:** `https://kubernetes.default.svc`
   * **Namespace:** `default`
4. Click **Create**.
5. Click **Sync** to deploy the application.

---

## Verify Deployment

### Check Pods

```bash
kubectl get pods -n default
```

Expected output:

```
nginx-deployment-xxxx   1/1   Running   0   2m
```

### Check Service

```bash
kubectl get svc -n default
```

Expected output:

```
nginx-service   ClusterIP   10.96.xx.xx   <none>   80/TCP   2m
```

### Access Nginx

**Option 1: Port-forward**

```bash
kubectl port-forward svc/nginx-service 8080:80 -n default
```

Then open in browser: `http://localhost:8080`

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
* Ensure your Kubernetes cluster is running before applying the manifests.

```

---

If you want, I can also **add a section with a live screenshot example of ArgoCD UI showing the synced Nginx app**, which makes the README look very professional for demos.  

Do you want me to add that?
```
