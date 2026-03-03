# Installation Guide

## Prerequisites

- Kubernetes cluster
- kubectl
- Helm
- Docker

---

## Step 1: Create Namespace

kubectl apply -f infrastructure/namespace.yaml

---

## Step 2: Install Prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack \
  -n observability \
  -f infrastructure/prometheus/values.yaml

---

## Step 3: Install Grafana

helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana \
  -n observability \
  -f infrastructure/grafana/values.yaml

---

## Step 4: Install Jaeger

helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm install jaeger jaegertracing/jaeger \
  -n observability \
  -f infrastructure/jaeger/values.yaml

---

## Step 5: Install EFK Stack

Install Elasticsearch:
helm install elasticsearch elastic/elasticsearch \
  -n observability \
  -f infrastructure/efk/elasticsearch-values.yaml

Deploy Fluentd:
kubectl apply -f infrastructure/efk/fluentd-daemonset.yaml

Install Kibana:
helm install kibana elastic/kibana \
  -n observability \
  -f infrastructure/efk/kibana-values.yaml

---

## Step 6: Install OpenTelemetry Collector

kubectl apply -f infrastructure/opentelemetry/otel-collector-config.yaml
kubectl apply -f infrastructure/opentelemetry/otel-collector-deployment.yaml

---

## Step 7: Deploy Sample Application

kubectl apply -f app/deployment.yaml
kubectl apply -f app/service.yaml