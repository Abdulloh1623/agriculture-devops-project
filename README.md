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

<img width="2559" height="1425" alt="image" src="https://github.com/user-attachments/assets/be0d34d8-e0c7-4821-9d59-707455178e63" />

### **Grafana Monitoring Hub**

> **Description:** This is the primary interface of the **Grafana Observability Stack** implemented for the Agricultural Platform. The sidebar displays a comprehensive list of pre-configured and custom-built dashboards, including the **Agricultural Platform - Executive View V2**, which serves as the central command center for both business and technical metrics.

**Key features visible in this view:**

* **Centralized Access:** Quick navigation to infrastructure health dashboards like *Node Exporter Full* and *Kubernetes / Views / Pods*.
* **Dashboard-as-Code:** The *Agricultural Platform* dashboard was deployed via a Kubernetes `ConfigMap`, ensuring the monitoring configuration is versioned and reproducible.
* **Operational Readiness:** The "Welcome to Grafana" status confirms that the data sources (Prometheus and Loki) are successfully integrated and ready for real-time data visualization.

<img width="2559" height="789" alt="image" src="https://github.com/user-attachments/assets/76d2b46c-b321-419b-a25c-f67cf16133d7" />

### **Integrated Observability Dashboard: Executive View**

> **Description:** This dashboard provides a comprehensive "single pane of glass" view into the **Agricultural Platform's** health and business operations. It integrates three critical observability dimensions: real-time business metrics, infrastructure traffic, and granular application logs.

**Key Monitoring Components:**

* **Business Logic Tracking (Scenario 1):** The "Products Updated Daily" panel monitors the core functional output of the microservices, tracking successful transaction completions across the cluster.
* **Real-time Traffic Analysis:** The "Real-time System Load" time-series graph visualizes incoming HTTP request rates, allowing for immediate identification of traffic spikes or potential bottlenecks.
* **Log Streaming (Scenario 5):** The "Application Logs" section provides a live feed of microservice activities. As shown in the view, it captures detailed HTTP status codes (e.g., 404 monitoring) and endpoint hits, which is essential for rapid debugging and security auditing.

<img width="2547" height="535" alt="image" src="https://github.com/user-attachments/assets/3297eddc-973b-4c63-8a9c-57106f9d507a" />

### **Integrated Observability Dashboard: Executive & Infrastructure View**

> **Description:** This dashboard provides a comprehensive "single pane of glass" view into both the **Agricultural Platform's** business operations and the underlying Kubernetes infrastructure health. It integrates critical observability dimensions: real-time business metrics, granular application logs, and global resource utilization.

**Key Monitoring Components:**

* **Business Logic Tracking (Scenario 1):** The "Products Updated Daily" panel monitors the core functional output of the microservices, tracking successful transaction completions across the cluster.
* **Real-time Traffic & System Load:** The "Real-time System Load" time-series graph visualizes incoming HTTP request rates, allowing for immediate identification of traffic spikes or potential bottlenecks.
* **Log Streaming (Scenario 5):** The "Application Logs" section provides a live feed of microservice activities. It captures detailed HTTP status codes and endpoint hits, essential for rapid debugging and security auditing.
* **Global Resource Infrastructure:** The "Kubernetes Resource Count" and "Global CPU/RAM Usage" panels provide a high-level overview of the cluster's health, displaying real-time consumption against defined requests and limits for all 26 running pods.

