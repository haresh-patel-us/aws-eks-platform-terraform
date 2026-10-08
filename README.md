# AWS EKS Platform on Terraform

Reference architecture showing how I design production-grade Amazon EKS
platforms with Terraform: VPC, managed node groups, IAM Roles for Service
Accounts (IRSA), and cluster add-ons (CoreDNS, VPC-CNI, EBS CSI, metrics-server).

This is a portfolio sample of the platform-engineering patterns I use daily —
remote-state backend, environment overlays, and add-on pinning. The code is
meant as a starting point, not a copy-paste production module.

## Layout

- `main.tf` — EKS cluster, node groups, add-ons
- `variables.tf` — inputs (cluster name, region, node sizing)
- `outputs.tf` — cluster endpoint, OIDC issuer, security groups
- `network.tf` — VPC + subnets (private for nodes, public for the API if desired)

## Usage

```bash
terraform init
terraform plan -var-file=envs/dev.tfvars
terraform apply -var-file=envs/dev.tfvars
```

## Patterns demonstrated

- Private node subnets with NAT egress; control-plane endpoint private-first
- IRSA via the cluster OIDC provider for workload identity
- Managed add-ons pinned to compatible versions
- Launch templates with IMDSv2 enforced and EBS encryption on
