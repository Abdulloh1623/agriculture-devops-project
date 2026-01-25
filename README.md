# agriculture-devops-project

## 📁 Project Structure

The repository is organized following best practices for Kubernetes-based microservices and GitOps workflows:

```bash
.
├── k8s/                         # Kubernetes manifests (The "Source of Truth")
│   ├── deployment.yaml          # Application deployment & pod configuration
│   ├── service.yaml             # ClusterIP service for internal networking
│   ├── servicemonitor.yaml      # Prometheus operator config for metrics scraping
│   ├── configmap.yaml           # Grafana dashboard JSON configuration
│   └── namespace.yaml           # Namespace definitions (argocd, default)
├── monitoring/                  # Monitoring stack configuration (Loki/Prometheus)
│   └── values.yaml              # Helm values for loki-stack customization
├── scripts/                     # Operational and testing utilities
│   └── load-generator.ps1       # PowerShell script for manual stress testing
├── app/                         # Application source code
│   ├── main.py                  # Microservice logic with Prometheus metrics
│   └── Dockerfile               # Container definition for the agriculture-app
└── README.md                    # Project documentation and portfolio showcase

```

### Component Descriptions:

* **`/k8s`**: Contains all declarative YAML files that **ArgoCD** monitors to maintain the cluster's desired state.
* **`servicemonitor.yaml`**: A critical DevOps component that allows Prometheus to auto-discover the application's metrics endpoint.
* **`configmap.yaml`**: Stores the Grafana dashboard JSON, allowing the dashboard to be deployed as code (Dashboard-as-Code).
* **`load-generator.ps1`**: Used for the final validation phase to ensure metrics and logs are correctly flowing to the observability stack.

---

## 🏗 Logical Architecture Diagram

To better understand how these files interact within the cluster:

1. **GitHub** stores the manifests.
2. **ArgoCD** pulls the manifests and applies them to **Kubernetes**.
3. **Promtail** and **Prometheus** collect data from the running **Pods**.
4. **Grafana** visualizes this data by querying **Loki** and **Prometheus**.
