# Project 13: Build Image and Push to ECR

This sample app is a static page served by Nginx. GitHub Actions builds its
Docker image and pushes it to Amazon ECR; Docker does not need to be installed
on your laptop.

## AWS and GitHub setup

1. In AWS, choose a region and create an ECR repository named `project13-app`.
2. Configure the GitHub OIDC identity provider in IAM if it is not already
   configured, then create an IAM role for this repository.
3. Give the role `ecr:GetAuthorizationToken` on `*`, and these permissions on
   the `project13-app` ECR repository:
   `ecr:BatchCheckLayerAvailability`, `ecr:CompleteLayerUpload`,
   `ecr:InitiateLayerUpload`, `ecr:PutImage`, `ecr:UploadLayerPart`.
4. Restrict the role's OIDC trust policy to this GitHub repository and the
   `main` branch:
   `repo:komal-ae-sharma/Project-13:ref:refs/heads/main`.
5. In GitHub, open **Settings > Secrets and variables > Actions** and add:
   - Repository variable `AWS_REGION`: the region containing the ECR repo.
   - Repository variable `ECR_REPOSITORY`: `project13-app`.
   - Repository secret `AWS_ROLE_ARN`: the IAM role ARN.
6. Push a commit to `main`, or start **Build and push to ECR** from the Actions
   tab. The workflow publishes both the commit SHA tag and `latest`.

The GitHub Actions runner builds the image. No local Docker installation is
needed.
