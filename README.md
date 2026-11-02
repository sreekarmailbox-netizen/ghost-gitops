# ghost-gitops
End-to-end AWS EKS GitOps project using Argo CD, Helm, Ghost, MySQL, Prometheus, Grafana and Fluent Bit.
# AWS EKS Ghost GitOps Project

## 1. Project Overview

This project deploys and manages the Ghost blogging platform on Amazon EKS using GitOps.

The platform uses Argo CD for continuous delivery, Helm for application deployment, Amazon EBS and EFS for persistent storage, Prometheus and Grafana for monitoring, and Fluent Bit with Splunk for centralized logging.

## 2. Architecture

### Cloud Infrastructure
- Amazon EKS
- Amazon EC2 managed node groups
- Amazon VPC and subnets
- AWS IAM and OIDC
- Amazon EBS CSI Driver
- Amazon EFS CSI Driver

### GitOps and Deployment
- GitHub
- Argo CD
- Helm
- Kubernetes
- Ghost
- MySQL

### Secrets Management
- AWS Systems Manager Parameter Store
- External Secrets Operator
- IAM Roles for Service Accounts (IRSA)

### Monitoring
- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- kube-state-metrics

### Logging
- Fluent Bit
- Splunk HTTP Event Collector
- Slack notifications (planned)

## 3. Storage Design

| Component | Storage | Purpose |
|---|---|---|
| MySQL | EBS gp3 | Database persistence |
| Prometheus | EBS gp3 | Metrics persistence |
| Ghost | EFS | Shared uploads |
| Fluent Bit | Node log paths | Container log collection |

## 4. GitOps Workflow

1. Developer updates the configuration in GitHub.
2. Changes are pushed to the main branch.
3. Argo CD detects repository changes.
4. The root application discovers child applications.
5. Argo CD deploys applications using Helm.
6. Kubernetes maintains the desired application state.
7. Prometheus collects metrics.
8. Grafana displays dashboards.
9. Fluent Bit collects container logs and forwards them to Splunk.
10. Alertmanager and Splunk can send notifications to Slack after integration.

## 5. Secret Management

Application credentials are stored in AWS Systems Manager Parameter Store as SecureString parameters.

External Secrets Operator retrieves credentials using IRSA and creates Kubernetes Secrets.

No passwords, access keys or Splunk tokens should be committed to GitHub.

## 6. Deployment Order

1. Prepare EKS and worker nodes.
2. Configure OIDC and IAM roles.
3. Install EBS CSI and EFS CSI drivers.
4. Create the EFS filesystem and mount targets.
5. Install Argo CD.
6. Configure External Secrets Operator.
7. Deploy infrastructure and storage.
8. Deploy MySQL.
9. Deploy Ghost.
10. Deploy Prometheus and Grafana.
11. Deploy Fluent Bit and connect Splunk.
12. Configure alerts and Slack integration.

## 7. AWS Configuration

Region: ap-south-1

Cluster: my-cluster

Repository: ghost-gitops

Branch: main

## 8. Security

- Use IAM roles instead of static AWS access keys.
- Use least-privilege IAM permissions.
- Store application credentials in Parameter Store.
- Enable encryption for EBS volumes.
- Restrict EFS access through security groups.
- Keep Grafana and Prometheus private.
- Use HTTPS for Ghost.
- Back up database and persistent volumes.

## 9. Validation

Check EKS nodes:

    kubectl get nodes

Check Argo CD applications:

    kubectl get applications -n argocd

Check application pods:

    kubectl get pods -n ghost
    kubectl get pods -n mysql
    kubectl get pods -n monitoring
    kubectl get pods -n logging
    kubectl get pods -n external-secrets

Check persistent volume claims:

    kubectl get pvc -A

Check storage classes:

    kubectl get storageclass

## 10. Project Objective

Build a secure, observable and GitOps-managed application platform on AWS EKS with automated deployments, persistent storage, centralized logging and monitoring.
