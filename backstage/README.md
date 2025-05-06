# Overview

An example to deploy Backstage demo on Kubernetes:
- Postgresql: CloudNative-PG
- GitOps: Argo CD
- Backstage chart

First, clone demo repository and build your own docker image


```bash
git clone https://github.com/backstage/demo
cd demo
docker image build . -t nvtienanh/backstage --build-arg ENVIRONMENT_CONFIG=production
docker push nvtienanh/backstage
```

# How to use

- Update SealedSecret then push
- `kubectl appply -f argocd.yaml`