# my-chart

A Helm chart repository for deploying the `my-app` application on Kubernetes.

## 🧰 Overview

This repository contains a Helm chart for deploying the `my-app` application on a Kubernetes cluster. The chart packages a deployment and service configuration for a containerized application, making it easy to install, manage, and scale using Helm.

## 🐳 Container image 
The container image is hosted on Docker Hub at [m4gapower/my_app](https://hub.docker.com/repository/docker/m4gapower/my_app/).


## 📦 Prerequisites

- Kubernetes cluster (version 1.19 or later)
- Helm 3.8+ installed (OCI support required for pulling charts from GitHub Container Registry)
- Access to a Kubernetes cluster with `kubectl` configured
- A GitHub Personal Access Token (PAT) with `read:packages` scope for accessing the chart from GitHub Container Registry

## 💻 Quick Start

### 1. Add the Helm Repository

To use the Helm chart hosted in the GitHub Container Registry, log in and add the repository:

```bash
helm registry login -u <your-github-username> ghcr.io --password <your-github-pat>
helm repo add my-chart oci://ghcr.io/ronthesoul/helm-charts
```

Replace `<your-github-username>` and `<your-github-pat>` with your GitHub username and Personal Access Token.

### 2. Install the Helm Chart

Install the `my-app` chart into your Kubernetes cluster:

```bash
helm install my-app oci://ghcr.io/ronthesoul/helm-charts/my-app --version 0.1.0
```

The chart is available at [ghcr.io/ronthesoul/helm-charts/my-app](https://github.com/ronthesoul/my-chart/pkgs/container/helm-charts%2Fmy-app).

### 3. Verify the Installation

Check the status of the deployment and service:

```bash
kubectl get deployments
kubectl get services
```

The `my-app` deployment runs a single replica of the `m4gapower/my_app:1.1.0` container, exposed via a `NodePort` service on port `8000`.

The Image registry: 

### 🔧 Configuration

The chart includes the following default configurations (defined in `values.yaml`):

- **Deployment**:
  - Replicas: 1
  - Container Image: `m4gapower/my_app:1.1.0`
  - Container Port: `8000`
  - Resources: Requests and limits set to `500Mi` memory and `500m` CPU
  - Security: Non-root user (`1001`), no privilege escalation, and `RuntimeDefault` seccomp profile
  - Readiness and Liveness Probes: HTTP checks on `/` at port `8000`

- **Service**:
  - Type: `NodePort`
  - Port: `8000`
  - Target Port: `8000`

To customize the deployment, create a `custom-values.yaml` file and override the defaults. For example:

```yaml
replicas: 3
resources:
  requests:
    cpu: "750m"
    memory: "750Mi"
  limits:
    cpu: "1500m"
    memory: "1000Mi"
```

Apply the custom configuration during installation:

```bash
helm install my-app oci://ghcr.io/ronthesoul/helm-charts/my-app --version 0.1.0 -f custom-values.yaml
```

### 🚀 Accessing the Application

To access the `my-app` service, find the `NodePort` assigned to the `my-app-service`:

```bash
kubectl get svc my-app-service
```

Use the cluster node's IP and the assigned `NodePort` (e.g., `http://<node-ip>:<node-port>`). Alternatively, if running locally (e.g., with `kube-2node`), use `kubectl port-forward` to access the service:

```bash
kubectl port-forward svc/my-app-service 8000:8000
```

Then, open `http://localhost:8000` in your browser.

### 🧪 Uninstalling the Chart

To remove the `my-app` deployment and service:

```bash
helm uninstall my-app
```

### 📁 Repository Structure

```
my-chart/
├── Chart.yaml               # Helm chart metadata
├── values.yaml              # Default configuration values
├── templates/
│   ├── deployment.yaml      # Kubernetes Deployment manifest
│   ├── service.yaml         # Kubernetes Service manifest
│   └── _helpers.tpl         # Helper templates for Helm
└── README.md                # This file
```

## 👤 Author

Written by Ron Negrov

## 📝 Notes

- Ensure the `m4gapower/my_app:1.1.0` container image is accessible in your container registry.
- If the chart is private, ensure your GitHub PAT has the necessary permissions (`read:packages`).
- For production use, consider configuring an Ingress resource or a LoadBalancer service instead of NodePort.
- To update the chart, modify the source in this repository, package it with `helm package .`, and push it to `ghcr.io/ronthesoul/helm-charts` using `helm push`.

For more details on Helm and chart management, visit [helm.sh](https://helm.sh) or [Artifact Hub](https://artifacthub.io).[](https://niklasmtj.de/blog/use-ghcr-to-host-helm-charts/)[](https://helm.sh/)
