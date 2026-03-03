# Installation Guide

## Prerequisites

- Kubernetes cluster
- kubectl
- Helm
- Docker

---

## 1. Create Namespace

```bash
kubectl apply -f infrastructure/namespace.yaml
```

## 2. Install Prometheus

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```bash
helm install prometheus prometheus-community/kube-prometheus-stack \
  -n observability \
  -f infrastructure/prometheus/custom-kube-prometheus-stack.yaml
  ```

## 3. Install Grafana

```bash
helm repo add grafana https://grafana.github.io/helm-charts
```
```bash
helm install grafana grafana/grafana \
  -n observability \
  -f infrastructure/grafana/values.yaml
  ```

## 4. Install Jaeger

```bash
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
```

```bash
helm install jaeger jaegertracing/jaeger \
  -n observability \
  -f infrastructure/jaeger/values.yaml
  ```

## 5. Deploy EFK

```bash
helm repo add elastic https://helm.elastic.co
```

```bash