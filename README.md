# ArgoCD Application Repository for Kubernetes

This repository hosts a collection of [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) application specifications designed to streamline the deployment and management of various services and tools on a Kubernetes cluster. By leveraging ArgoCD's GitOps capabilities, this repository provides a declarative way to maintain your cluster's desired state, ensuring consistency, repeatability, and traceability of your deployments.

**Benefits:**
* **GitOps Workflow:** Manage your Kubernetes applications declaratively through Git.
* **Automated Sync:** ArgoCD automatically syncs the applications to your cluster when changes are pushed to this repository.
* **Version Control:** Keep track of all configuration changes and easily roll back if needed.
* **Consistency:** Ensure that your applications are deployed in a consistent manner across different environments.
* **Pre-configured Applications:** Quickly deploy commonly used Kubernetes tools and services.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Available Applications](#available-applications)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

## Prerequisites

Before you can deploy these applications using ArgoCD, ensure you have the following:

*   **A running Kubernetes cluster:** You'll need a Kubernetes cluster (e.g., Minikube, Kind, GKE, EKS, AKS).
*   **`kubectl` installed and configured:** Ensure `kubectl` is installed and configured to communicate with your cluster. You can find installation instructions [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
*   **ArgoCD installed on your cluster:** ArgoCD must be installed in your cluster and accessible. Follow the [official ArgoCD documentation](https://argo-cd.readthedocs.io/en/stable/getting_started/) for installation instructions.
*   **Git:** You need Git installed to clone this repository or to allow ArgoCD to access it.

## Available Applications

This repository provides ArgoCD configurations for deploying the following applications:

-   **[Sealed Secrets](./sealed-secrets/argocd.yaml)**: Manages Kubernetes Secrets by encrypting them into "SealedSecrets" which are safe to store in public Git repositories. ([Official Documentation](https://github.com/bitnami-labs/sealed-secrets#sealed-secrets))
-   **[Metrics Server](./metrics-server/argocd.yaml)**: Collects resource metrics from Kubelets and exposes them in the Kubernetes API server through `metrics.k8s.io`. ([Official Documentation](https://github.com/kubernetes-sigs/metrics-server#kubernetes-metrics-server))
-   **[Dynamic NFS Provisioning](./nfs-storage/argocd.yaml)**: An automatic provisioner that uses an existing NFS server to provide dynamic provisioning of PersistentVolumes. ([Official Documentation](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner#nfs-subdir-external-provisioner))
-   **[Kiali Operator](./kiali-operator/argocd.yaml)**: Provides observability for your Istio service mesh, offering detailed metrics, tracing, and visualization. ([Official Documentation](https://kiali.io/docs/installation/installation-guide/operator/))
-   **[Ingress Nginx](./ingress-nginx/argocd.yaml)**: An Ingress controller for Kubernetes using NGINX as a reverse proxy and load balancer. ([Official Documentation](https://kubernetes.github.io/ingress-nginx/))
-   **[Kube Prometheus Stack](./monitoring/argocd.yaml)**: Provides a comprehensive monitoring solution for Kubernetes clusters, including Prometheus, Grafana, and Alertmanager. ([Official Documentation](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#kube-prometheus-stack))
-   **[NFS CSI driver](./csi-nfs-driver/argocd.yaml)**: A Container Storage Interface (CSI) driver for NFS that allows dynamic provisioning of NFS volumes. ([Official Documentation](https://github.com/kubernetes-csi/csi-driver-nfs#nfs-csi-driver-for-kubernetes))
-   **[CloudNative-PG](./cloudnative-pg/argocd.yaml)**: Operator for deploying and managing PostgreSQL databases on Kubernetes, following cloud-native principles. ([Official Documentation](https://cloudnative-pg.io/documentation/stable/))
-   **[Backstage](./backstage/argocd.yaml)**: An open platform for building developer portals, centralizing infrastructure tooling, software components, and documentation. ([Official Documentation](https://backstage.io/docs/overview/what-is-backstage/))

## Usage

This repository is designed to be used as a source for ArgoCD applications.

### Connecting to ArgoCD

1.  **Add this Repository to ArgoCD:**
    In the ArgoCD UI, navigate to `Settings` -> `Repositories` and add this Git repository.
    Alternatively, you can use the ArgoCD CLI:
    ```bash
    argocd repo add https://github.com/nvtienanh/argocd-applications --name nvtienanh-argocd-apps
    ```
    (Replace the repository URL if you are using a fork).

2.  **Deploying an Application:**
    Once the repository is connected, you can deploy any application listed in the `Available Applications` section.
    *   **Using the ArgoCD UI:** Click on `New App`, select the connected repository, specify the path to the application's directory (e.g., `sealed-secrets`, `monitoring`), and configure the destination cluster and namespace.
    *   **Using the ArgoCD CLI:** You can create an ArgoCD application manifest that points to a path in this repository. For example, to deploy Sealed Secrets:
        ```yaml
        apiVersion: argoproj.io/v1alpha1
        kind: Application
        metadata:
          name: my-sealed-secrets
          namespace: argocd
        spec:
          project: default
          source:
            repoURL: https://github.com/nvtienanh/argocd-applications # Or your fork's URL
            path: sealed-secrets # Path to the application directory
            targetRevision: HEAD
          destination:
            server: "https://kubernetes.default.svc"
            namespace: kube-system # Target namespace for Sealed Secrets
          syncPolicy:
            automated:
              prune: true
              selfHeal: true
        ```
        Save this as `my-sealed-secrets-app.yaml` and apply it:
        ```bash
        kubectl apply -f my-sealed-secrets-app.yaml -n argocd
        ```

### Customization

*   **Forking:** For significant customizations or to manage your own configurations, it is recommended to fork this repository. You can then modify the Helm chart parameters or Kubernetes manifests within your fork.
*   **Parameters:** Some applications might expose parameters directly in their `argocd.yaml` files (e.g., under `source.helm.parameters`). Review the specific `argocd.yaml` for an application to see if direct parameter overrides are suitable for your needs before considering a fork. For deeper changes, forking is the preferred approach.

## Contributing

Contributions are welcome! If you have improvements, bug fixes, or new applications to add, please follow these steps:

1.  **Fork the Repository:** Create your own fork of the repository.
2.  **Create a Branch:** Make your changes in a new git branch:
    ```bash
    git checkout -b my-feature-branch main
    ```
3.  **Make your Changes:** Implement your feature or bug fix.
4.  **Test your Changes:** Ensure your changes work as expected and do not break existing functionality.
5.  **Commit your Changes:** Commit your changes with a clear commit message:
    ```bash
    git commit -m "feat: Add new cool feature"
    ```
6.  **Push to your Fork:** Push your changes to your forked repository:
    ```bash
    git push origin my-feature-branch
    ```
7.  **Submit a Pull Request:** Open a pull request from your fork to the main repository. Provide a clear description of your changes.

If you're adding a new application, please ensure it follows the existing structure and include a brief README within its directory if necessary.

## License

This repository and its contents are licensed under the [GNU General Public License v3.0](./LICENSE). See the `LICENSE` file for more details.

---

## Author

Anh Nguyễn
- Website: [https://nvtienanh.info](https://nvtienanh.info)
- Contact: [me@nvtienanh.info](mailto:me@nvtienanh.info)