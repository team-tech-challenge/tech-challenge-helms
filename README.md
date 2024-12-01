# tech-challenge-helms

[![Maintained?](https://img.shields.io/badge/Maintained%3F-yes-green.svg)]()
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![ArgoCD](https://img.shields.io/badge/ArgoCD-2.12.3-blue.svg)](https://argo-cd.readthedocs.io/en/stable/)
[![EKS](https://img.shields.io/badge/EKS-1.30-blue.svg)](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)

This is repository stores helm manifests for use with ArgoCD


## Table of Contents
- [Prerequisites](#prerequisites)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [References](#references)

---

### Prerequisites

How to prerequisites for utilization of this repository.

1. A GitHub account.

2. Access to the ArgoCD instances for each environment (DEV, STG, PROD):
    - **PROD**: [https://argocd.techchallenge.com.br](https://argocd.techchallenge.musa.co)

3. `kubectl` installed on your machine. You can install it by following the [official guide](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

4. Access to the Kubernetes cluster where ArgoCD is installed.

---

### Usage

How to use this repository.

1. Clone this repository:

```bash
git clone https://github.com/yourusername/tech-challenge-helm.git
```

2. Change to the repository directory:

```bash
cd tech-challenge-helms
```

3. Create a new branch with a descriptive name:

```bash
git checkout -b my-feature-branch
```

4. Create a new directory for your application:

```bash
cd apps && mkdir my-app
```

***Example***:

- Create a new directory for install application `kube-prometheus-stack`:

```bash
cd apps && mkdir kube-prometheus-stack
``` 

5. Create a new `values.yaml` file with the values for your application:

```bash
cd kube-prometheus-stack && touch values.yaml
```

6. Add the values for your application to the `values.yaml` file:

```yaml
# values.yaml
prometheus:
  enabled: true
  alertmanager:
    enabled: true
  nodeExporter:
    enabled: true
  kubeStateMetrics:
    enabled: true
  grafana:
    enabled: true
  pushgateway:
    enabled: true
```

7. Create a new `Chart.yaml` file with the metadata for your application:

```bash
touch Chart.yaml
```

8. Add the metadata for your application to the `Chart.yaml` file:

```yaml
# Chart.yaml
apiVersion: v2
name: kube-prometheus-stack
description: A Helm chart for the kube-prometheus-stack application
version: 0.1.0
```

9. Create a new yaml file for your application:

```bash
cd apps/app-of-apps && touch kube-prometheus-stack-app.yaml
```

```yaml
# kube-prometheus-stack-app.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: kube-prometheus-stack
  namespace: argocd
spec:
    project: default
    source:
        repoURL: 'https://github.com/Musatech/apps-helm-default.git'
        targetRevision: HEAD
        path: apps/kube-prometheus-stack
        helm:
           values: |
              prometheus:
                enabled: true
                alertmanager:
                  enabled: true
                nodeExporter:
                  enabled: true
                kubeStateMetrics:
                  enabled: true
                grafana:
                  enabled: true
                pushgateway:
                  enabled: true
    destination:
        server: 'https://kubernetes.default.svc'
        namespace: monitoring
    syncPolicy:
        automated:
            prune: true
            selfHeal: true
        syncOptions:
            - CreateNamespace=true
            - PruneLast=true 
            - ApplyOutOfSyncOnly=true
            - ServerSideApply=true
```

10. Commit your changes:

```bash
git add .
git commit -m "Add kube-prometheus-stack application"
git push origin my-feature-branch
```

11. Open a pull request to the main repository.
12. Wait for the pull request to be reviewed and merged.
13. Check the ArgoCD instance for the environment where you want to deploy the application.
14. Sync the application to deploy it.
15. Check the application status in the ArgoCD instance.

---

### Contributing

We welcome contributions! Please follow the steps below to contribute to this repository:

1. Fork the repository.
2. Create a new branch with a descriptive name:
   ```bash
   git checkout -b my-feature-branch
   ```
3. Make your changes and commit them with a meaningful message:
   ```bash
   git commit -m "Add my new feature"
   ```
4. Push your branch to your forked repository:
   ```bash
   git push origin my-feature-branch
   ```
5. Open a pull request to the main repository.

For more details, refer to our [Contributing Guidelines](CONTRIBUTING.md).

---

### License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

---

### References

- [ArgoCD Documentation](https://argo-cd.readthedocs.io/en/stable/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Amazon EKS Documentation](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
- [kubectl Documentation](https://kubernetes.io/docs/reference/kubectl/)

---

