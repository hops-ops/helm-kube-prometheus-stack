# helm-kube-prometheus-stack

A Crossplane Configuration package that installs the kube-prometheus-stack Helm chart with a minimal, stable interface.

## Overview

`helm-kube-prometheus-stack` renders a single Helm release for kube-prometheus-stack. It exposes only the inputs needed
for chart values, namespace, and release name, keeping the interface stable while allowing full Helm overrides.

## Features

- **Minimal Helm interface**: values and overrideAllValues passed directly to Helm
- **Predictable naming**: defaults to `<clusterName>-kube-prometheus-stack` in the `monitoring` namespace
- **GitOps friendly**: ships a `.gitops/` deploy chart

## Prerequisites

- Crossplane installed in the cluster
- Crossplane providers:
  - `provider-helm` (>=v1.0.2)
- Crossplane function:
  - `function-auto-ready` (>=v0.6.0)

## Quick Start

```yaml
apiVersion: pkg.crossplane.io/v1
kind: Configuration
metadata:
  name: helm-kube-prometheus-stack
spec:
  package: ghcr.io/hops-ops/helm-kube-prometheus-stack:latest
```

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: KubePrometheusStack
metadata:
  name: kube-prometheus-stack
  namespace: example-env
spec:
  clusterName: example-cluster
  values:
    grafana:
      adminPassword: admin
    prometheus:
      prometheusSpec:
        retention: 7d
```

## Development

```bash
make render
make validate
make test
```
