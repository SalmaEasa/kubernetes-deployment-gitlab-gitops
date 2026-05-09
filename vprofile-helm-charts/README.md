# vprofile-helm-charts

Helm charts and GitLab CI/CD pipeline for deploying the vProfile application to AWS EKS.

## Overview

This repository is part of a 3-pipeline GitOps workflow:

1. **Infrastructure Pipeline** - Terraform code to provision AWS EKS cluster and resources
2. **CI Pipeline** - [vprofile-java-app](https://gitlab.com/devops5964419/vprofile-java-app) builds Docker images and pushes to GitLab Container Registry
3. **CD Pipeline** - This repository deploys the application to EKS using Helm charts

## Pipeline Stages

- **lint** - Validates Helm chart syntax
- **package** - Packages the Helm chart
- **deploy** - Deploys to EKS cluster using Helm
- **drift-check** - Detects configuration drift between Git and cluster state

## Prerequisites

- AWS EKS cluster (vprofile-eks-cluster)
- GitLab Runner with `ec2` tag
- GitLab Deploy Token with `read_registry` scope from vprofile-java-app project

## Required CI/CD Variables

Set these in GitLab project settings:

- `DOCKER_REGISTRY_SERVER` - registry.gitlab.com
- `DOCKER_REGISTRY_USERNAME` - Deploy token username
- `DOCKER_REGISTRY_PASSWORD` - Deploy token password

## Deployment

Push to main branch to trigger automatic deployment:

```bash
git add .
git commit -m "Update configuration"
git push origin main
```

## Application Components

- **vproapp** - Java web application (Tomcat)
- **vprodb** - MySQL database
- **vprocache01** - Memcached
- **vpromq01** - RabbitMQ

## Helm Chart

Located in `vprofile-chart/` directory with customizable values in `values.yaml`.
