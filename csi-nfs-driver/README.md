
# Install NFS CSI driver with Argo CD

This is a repository for NFS CSI driver, csi plugin name: `nfs.csi.k8s.io`. This driver requires existing and already configured NFSv3 or NFSv4 server, it supports dynamic provisioning of Persistent Volumes via Persistent Volume Claims by creating a new sub directory under NFS server.

```bash
kubectl apply -f argocd.yaml
```

[NFS CSI driver for Kubernetes](https://github.com/kubernetes-csi/csi-driver-nfs)