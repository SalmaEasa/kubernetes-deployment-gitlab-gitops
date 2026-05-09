# EKS Infrastructure with Terraform

[![Pipeline Status](https://gitlab.com/devops5964419/infra-eks-terraform/badges/main/pipeline.svg)](https://gitlab.com/devops5964419/infra-eks-terraform/-/pipelines)

> Terraform configuration for AWS EKS cluster with GitLab CI/CD

## What This Does

Creates an AWS EKS cluster with:
- VPC with public subnets (cost-optimized)
- EKS cluster: `vprofile-eks-cluster`
- Node group: 1x t3.small instance
- EBS CSI driver for persistent storage
- Automated GitLab CI/CD pipeline

## Quick Start

### 1. Set GitLab Variables
Go to **Settings → CI/CD → Variables** and add:
```
AWS_ACCESS_KEY_ID = your-aws-access-key
AWS_SECRET_ACCESS_KEY = your-aws-secret-key
```

### 2. Deploy
```bash
# Create MR with any change
git checkout -b deploy
echo "deploy" >> README.md
git add . && git commit -m "Deploy EKS"
git push origin deploy

# Create MR → Pipeline runs → Merge → Manual apply
```

### 3. Access Cluster
```bash
aws eks update-kubeconfig --region us-east-1 --name vprofile-eks-cluster
kubectl get nodes
```

## Files

- `main.tf` - Infrastructure resources
- `variables.tf` - Configuration options
- `outputs.tf` - Cluster information
- `.gitlab-ci.yml` - CI/CD pipeline

## Pipeline Stages

1. **validate** - Check Terraform syntax (MR only)
2. **plan** - Show what will be created (MR only)
3. **apply** - Create infrastructure (manual, main branch)
4. **destroy** - Delete everything (manual, safety checks)

## Destroy

1. Set `DESTROY_MODE=true` in GitLab variables
2. Commit with message containing `DESTROY_CLUSTER`
3. Manually trigger destroy job

## Related Projects

This is part of a 3-pipeline workflow:
- **Infrastructure** (this repo) → **CI** ([vprofile-java-app](../vprofile-java-app)) → **CD** ([vprofile-helmchart](../vprofile-helmchart))

## Cost

~$75-100/month (cluster + node + storage)