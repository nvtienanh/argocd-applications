# NFS Storage Provisioner - Argo CD Application

This repository contains the Argo CD Application manifest for deploying the [NFS Subdir External Provisioner](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner) in a Kubernetes cluster using GitOps principles.

## Overview

This Helm chart deploys an automatic provisioner for Kubernetes that uses your existing NFS server to support dynamic provisioning of PersistentVolumes via PersistentVolumeClaims.

## Application Configuration

### Key Features
- **Dynamic Provisioning**: Automatically creates PVs when PVCs are requested
- **Subdirectory Isolation**: Each PVC gets its own subdirectory on the NFS share
- **Storage Retention**: Configured to retain data when PVCs are deleted
- **GitOps Managed**: Fully declarative configuration via Argo CD

### Helm Chart Parameters
| Parameter | Value | Description |
|-----------|-------|-------------|
| `nfs.server` | `192.168.1.11` | Your NFS server IP/hostname |
| `nfs.path` | `/mnt/kubernetes-volumes/data` | Base NFS export path |
| `storageClass.name` | `nfs-storage` | Name of the StorageClass |
| `storageClass.onDelete` | `retain` | Retention policy for volumes |
| `storageClass.pathPattern` | `/\\$\\{\\.PVC\\.namespace\\}-\\$\\{\\.PVC\\.name\\}` | Dynamic path pattern for PVs |

## Deployment

### Prerequisites
- Argo CD installed in your cluster
- NFS server accessible from the Kubernetes cluster
- Appropriate network/firewall rules for NFS (typically port 2049)

### Installation
1. Apply the manifest:
   ```bash
   kubectl apply -f nfs-storage-application.yaml -n argocd
   ```
