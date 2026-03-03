# Installation Guide

## Prerequisites

- Kubernetes cluster
- kubectl
- Helm
- Docker

---

## 1. Create Namespace

kubectl apply -f infrastructure/namespace.yaml

## 2. Install Prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm install prometheus prometheus-community/kube-prometheus-stack \
  -n observability \
  -f infrastructure/prometheus/custom-kube-prometheus-stack.yaml

