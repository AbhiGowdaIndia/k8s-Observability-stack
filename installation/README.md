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

## 3. Install Grafana

helm repo add grafana https://grafana.github.io/helm-charts

helm install grafana grafana/grafana \
  -n observability \
  -f infrastructure/grafana/values.yaml

## 4. Install Jaeger

helm repo add jaegertracing https://jaegertracing.github.io/helm-charts

helm install jaeger jaegertracing/jaeger \
  -n observability \
  -f infrastructure/jaeger/values.yaml

## 5. Deploy EFK

helm repo add elastic https://helm.elastic.co

helm install elasticsearch elastic/elasticsearch \
  -n observability \
  -f infrastructure/efk/elasticsearch-values.yaml

kubectl apply -f infrastructure/efk/fluentd-daemonset.yaml

helm install kibana elastic/kibana \
  -n observability \
  -f infrastructure/efk/kibana-values.yaml

## 6. Deploy OpenTelemetry

kubectl apply -f infrastructure/opentelemetry/

## 7. Deploy Application

kubectl apply -f app/