# VProfile Java Application - CI Pipeline

This repository contains the Java application source code and CI pipeline for the VProfile project.

## Overview

This is part of a 3-pipeline GitOps workflow:
1. **Infrastructure Pipeline** - Terraform for AWS infrastructure provisioning
2. **Application Pipeline** (this repo) - Java app build, test, and Docker image creation
3. **Deployment Pipeline** - Helm chart updates and Kubernetes deployments

## Technologies

- **Backend**: Spring MVC, Spring Security, Spring Data JPA
- **Database**: MySQL 8
- **Build Tool**: Maven 3
- **Runtime**: JDK 11, Tomcat
- **Additional Services**: Memcached, RabbitMQ, ElasticSearch

## Pipeline Stages

1. **build** - Maven clean install (skip tests)
2. **unit-test** - Run tests and generate reports
3. **checkstyle** - Static code analysis
4. **sonar-analysis** - SonarCloud quality scan
5. **quality-gate** - Verify SonarCloud quality gate
6. **docker-build** - Build and push Docker image
7. **update-deployment** - Update Helm chart with new image

## Prerequisites

- JDK 11
- Maven 3
- MySQL 8

## Database Setup

Import the SQL dump file:
```bash
mysql -u <username> -p accounts < src/main/resources/db_backup.sql
```

## CI/CD Variables Required

- `LOGIN` - SonarCloud token
- `HOST` - SonarCloud URL
- `ORGANIZATION` - SonarCloud organization
- `PROJECT` - SonarCloud project key
- `CHARTS_REPO_URL` - Helm charts repository URL
- `CHARTS_REPO_TOKEN` - Git token for charts repo

## Triggers

- **Merge Requests**: Runs build, test, analysis, and Docker build
- **Main Branch**: Runs full pipeline including deployment updates