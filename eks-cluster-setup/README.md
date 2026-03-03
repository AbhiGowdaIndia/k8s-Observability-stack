# AWS EKS Cluster Setup Guide

This document explains how to create and configure an Amazon EKS cluster 
for deploying the Kubernetes Observability Demo project.

---

## 📌 Overview

We will create:

- An Amazon EKS cluster
- Managed node group
- IAM role configuration
- kubectl access configuration

Cluster will be created in:

Region: ap-south-1

---

## 🧰 Prerequisites

Make sure the following tools are installed:

- AWS CLI (configured with IAM permissions)
- eksctl
- kubectl
- Helm

Verify installations:

aws --version  
eksctl version  
kubectl version --client  

---

## 🌩 Step 1: Create EKS Cluster

Create cluster using eksctl:
 
```bash
eksctl create cluster \
--name=observability-cluster \
--region=ap-south-1 \
--zone=ap-south-1a,ap-south-1b \
--without-nodegroup
```
* Creates a EKS cluster with name observability-cluster in ap-south-1 region without node group.

##  step 2: Create and link OIDC Identity provider 

```bash
eksctl utils associate-iam-oidc-provider \
  --region=ap-south-1 \
  --cluster=observability-cluster \
  --approve
```
* Creates an OIDC identity provider in AWS IAM and links it to our EKS cluster in ap-south-a region.

## step 3: Create node group to our cluster

```bash
eksctl create nodegroup \
  --cluster=observability-cluster \
  --region=ap-south-1 \
  --name=observability-nodes \
  --node-type=t3.medium \
  --nodes-min=2 \
  --nodes-max=3 \
  --node-volume-size=20 \
  --managed \
  --asg access \
  --external-dns-access \
  --full-ecr-access \
  --appmess-access \
  --alb-ingress-access \
  --node-private-networking
```
* creates a worker node group with name "observability-nodes" for our existing "observability-cluster".
* It provisions EC2 instances of type t3.medium with a minimum of 2 nodes and allows scaling up to 3 nodes using an Auto Scaling Group, each node having a 20 GB EBS volume for storage.
* The --managed flag ensures AWS manages lifecycle tasks like upgrades and patching.

## step 4: updates our local kubeconfig file

```bash
aws eks update-kubeconfig --name observability-cluster
```
* Updates our local kubeconfig file (usually located at ~/.kube/config) so that kubectl can connect to your Amazon EKS cluster named "observability-cluster".
* In simple terms, this command tells our local Kubernetes CLI how to authenticate and communicate with your EKS cluster in AWS.

## step 4: Verify Cluster

```bash
kubectl get nodes
```
