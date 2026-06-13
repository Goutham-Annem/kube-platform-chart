# kube-platform

> One Helm chart to bootstrap your entire EKS platform stack.

[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/kube-platform)](https://artifacthub.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Installs and wires together:

| Component | Purpose |
|-----------|---------|
| **cert-manager** | Automated TLS (Let's Encrypt / ACM) |
| **external-dns** | Route53 record sync from Ingress/Service |
| **karpenter** | Event-driven node provisioning |
| **metrics-server** | `kubectl top` + HPA metrics |

## Install

```bash
helm repo add kube-platform https://goutham-annem.github.io/kube-platform-chart
helm repo update

helm install platform kube-platform/kube-platform \
  --namespace kube-system \
  --set global.clusterName=my-cluster \
  --set global.region=us-east-1 \
  --set global.accountId=123456789012 \
  --set cert-manager.serviceAccount.annotations."eks\.amazonaws\.com/role-arn"=arn:aws:iam::123456789012:role/cert-manager \
  --set external-dns.serviceAccount.annotations."eks\.amazonaws\.com/role-arn"=arn:aws:iam::123456789012:role/external-dns \
  --set karpenter.serviceAccount.annotations."eks\.amazonaws\.com/role-arn"=arn:aws:iam::123456789012:role/karpenter
```

## Custom values

```bash
helm show values kube-platform/kube-platform > my-values.yaml
# Edit my-values.yaml, then:
helm install platform kube-platform/kube-platform -f my-values.yaml
```

## Disable components

```bash
helm install platform kube-platform/kube-platform \
  --set karpenter.enabled=false   # use Cluster Autoscaler instead
  --set external-dns.enabled=false # if you manage DNS elsewhere
```

## License

MIT — by [Goutham Annem](https://linkedin.com/in/goutham-annem)
