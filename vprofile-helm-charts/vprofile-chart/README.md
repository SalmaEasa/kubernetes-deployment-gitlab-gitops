# VProfile Helm Chart

This Helm chart deploys the VProfile application stack with support for pulling images from GitLab Container Registry.

## GitLab Container Registry Authentication

This chart supports pulling images from GitLab Container Registry using credentials stored in GitLab CI/CD variables.

### Required GitLab CI/CD Variables

Set the following variables in your GitLab project's CI/CD settings:

- `DOCKER_REGISTRY_USERNAME`: Your GitLab username or deploy token username
- `DOCKER_REGISTRY_PASSWORD`: Your GitLab personal access token or deploy token
- `DOCKER_REGISTRY_SERVER`: GitLab registry URL (default: registry.gitlab.com)

### How it works

The chart creates a Kubernetes Secret of type `kubernetes.io/dockerconfigjson` that contains the authentication information needed to pull images from the GitLab Container Registry.

## Usage

1. Set the required GitLab CI/CD variables in your GitLab project settings
2. Deploy the chart using GitLab CI/CD:

```yaml
deploy:
  stage: deploy
  script:
    - helm upgrade --install vprofile ./vprofile-chart
```

## Configuration

See the `values.yaml` file for all available configuration options.