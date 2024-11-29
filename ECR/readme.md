# AWS Elastic Container Registry (ECR) Guide

This repository provides a comprehensive guide to using **AWS Elastic Container Registry (ECR)** for storing, managing, and deploying container images. AWS ECR is a fully managed container registry service that integrates seamlessly with other AWS services such as ECS, EKS, and Lambda.

---

## Features

- **Private and Public Repositories**: Store container images securely or share them globally.
- **Seamless AWS Integration**: Works with Amazon ECS, EKS, and AWS Fargate.
- **Image Scanning**: Detect vulnerabilities in your container images.
- **Lifecycle Policies**: Automatically manage image retention to optimize storage.
- **Scalability and High Availability**: Handle concurrent access with ease.

---

## Workflow Overview

1. **Create a Repository**:
   - Use the AWS Management Console, CLI, or SDKs to create a private or public repository.

2. **Authenticate Docker CLI**:
   - Retrieve an authentication token using:
     ```bash
     aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
     ```

3. **Push Images**:
   - Tag the local Docker image:
     ```bash
     docker tag <image_name>:<tag> <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
     ```
   - Push the image to ECR:
     ```bash
     docker push <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
     ```

4. **Pull Images**:
   - Pull images from ECR:
     ```bash
     docker pull <account_id>.dkr.ecr.<region>.amazonaws.com/<repository_name>:<tag>
     ```

5. **Deploy Images**:
   - Use the stored images with Amazon ECS, EKS, or Kubernetes.

---

## Best Practices

- **Enable Image Scanning**: Regularly scan images for vulnerabilities.
- **Use Lifecycle Policies**: Automatically clean up unused images.
- **Implement Least Privilege Access**: Use IAM policies to restrict access.
- **Tag Images Appropriately**: Use meaningful and consistent version tags.
- **Monitor Activity**: Utilize CloudWatch for logging and monitoring.

---

## Common Use Cases

- **Microservices**: Store and manage container images for microservices in ECS/EKS.
- **CI/CD Pipelines**: Integrate ECR into your CI/CD workflows for automated deployments.
- **Public Image Hosting**: Share container images for open-source or global use.

---

## Example Commands

### Creating a Repository
```bash
aws ecr create-repository --repository-name <repository_name> --region <region>

# Authenticating Docker CLI 
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com

#Tagging and Pushing an Image
docker tag my-image:latest <account_id>.dkr.ecr.<region>.amazonaws.com/my-repository:latest
docker push <account_id>.dkr.ecr.<region>.amazonaws.com/my-repository:latest

#Pulling an Image
docker pull <account_id>.dkr.ecr.<region>.amazonaws.com/my-repository:latest

