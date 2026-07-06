# Deployment Guide - Docker React Vite

This document describes the automated deployment workflow using GitHub Actions and AWS Elastic Beanstalk.

## Workflow
1. **Development**: All work must be performed on a specific `feature` branch.
2. **Pull Request**: Once the feature is complete, open a Pull Request to the `main` branch.
3. **Automated Deployment**: After merging into `main`, GitHub Actions executes the deployment pipeline:
    - Docker image build (`Dockerfile.dev`).
    - Automated testing execution.
    - Project compression (`zip`).
    - Deployment to AWS Elastic Beanstalk.

## Required Configuration (Secrets)
To ensure the pipeline functions correctly, the following variables must be configured in your GitHub repository (under **Settings > Secrets and variables > Actions**):
- `AWS_ACCESS_KEY`: Access key from the IAM user (Required permission: `AdministratorAccess-AWSElasticBeanstalk`).
- `AWS_SECRET_KEY`: Secret access key from the IAM user.
- `DOCKER_USERNAME`: Your Docker Hub username.
- `DOCKER_PASSWORD`: Your Docker Hub access token.

## Environment Recreation (In case of failure)
1. Create an IAM Role (`aws-elasticbeanstalk-ec2-role`) with `WebTier` and `WorkerTier` policies.
2. Create an Elastic Beanstalk Environment (Platform: Docker on Amazon Linux 2).
3. Configure S3 bucket permissions (Object Ownership: set to **ACLs enabled**).