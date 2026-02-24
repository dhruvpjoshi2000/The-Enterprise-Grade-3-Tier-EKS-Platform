# The-Enterprise-Grade-3-Tier-EKS-Platform
# 🚀 Zero-Touch EKS Provisioning

This repository contains the Infrastructure as Code (Terraform) and GitOps configurations (Argo CD) to provision a production-grade Kubernetes cluster on AWS.

## 🛠️ Tech Stack

| Category | Technology | Description |
| :--- | :--- | :--- |
| **Cloud** | AWS | Host provider (VPC, IAM, EKS) |
| **IaC** | Terraform | Provisions VPC, Cluster, Node Groups |
| **GitOps** | Argo CD | Continuous Deployment of applications |
| **Ingress** | NGINX + NLB | Layer 4/7 Traffic handling |
| **DNS** | ExternalDNS | Syncs Ingress/Service IPs to Route53 |
| **Security** | Cert-Manager | Automates Let's Encrypt SSL certificates |
| **Scaling** | Karpenter | Just-in-time node provisioning |

## 🛡️ Cluster Security Hardening

To ensure a "Secure by Design" architecture, the following measures are implemented automatically via Terraform:

1.  **Least Privilege Access (IRSA):**
    * Pods do not use hardcoded AWS keys.
    * **ExternalDNS**, **Cert-Manager**, and **Karpenter** utilize OIDC-federated IAM Roles.
2.  **Private Networking:**
    * EKS Control Plane Endpoint is restricted.
    * Nodes reside in private subnets; only Load Balancers are public.
3.  **RBAC (Role-Based Access Control):**
    * `aws-auth` is managed to strictly map IAM roles to Kubernetes groups.
    * Developers have `view` access; CI/CD pipelines have `edit` access.
4.  **Security Groups:**
    * Strict Inbound/Outbound rules applied to Worker Nodes.

## 🔄 Traffic Flow Diagram

`[User] -> [Route53 (DNS)] -> [AWS Network Load Balancer] -> [Ingress-Nginx Controller] -> [Service] -> [Pod]`

* **SSL Offloading:** Handled at the Ingress level via Cert-Manager.
* **Routing:** Path-based routing defined in Ingress resources.