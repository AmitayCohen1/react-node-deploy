# CI/CD for React Vite to S3 with CloudFront Invalidation

This guide covers the process of setting up a CI/CD pipeline to automatically deploy your React Vite application to an AWS S3 bucket with CloudFront invalidation using GitHub Actions.

## Prerequisites

1. **GitHub Repository**: This guide assumes you have a GitHub repository where your React Vite application code is stored.

2. **AWS Account**: You must have an AWS account with permissions to create and configure S3 buckets and CloudFront distributions.

3. **IAM Role**: Create an IAM role in AWS with the necessary permissions to access S3 and CloudFront. This role will be assumed by GitHub Actions. Ensure you have the IAM role ARN.

4. **GitHub Secrets**: Store sensitive information (IAM role's access keys and other secrets) as GitHub secrets in your GitHub repository.

5. **AWS Region**: Determine the AWS region where your S3 bucket and CloudFront distribution will be located.

6. **CloudFront Distribution ID**: Create a CloudFront distribution in your AWS account and note down the Distribution ID.

## Steps

### 1. Setup Secrets in GitHub

Your GitHub repository will use secrets to store sensitive information that GitHub Actions will use. Here's how to set them up:

1. Navigate to your GitHub repository.

2. Click on the "Settings" tab.

3. Under "Settings," click on "Secrets."

4. Click on "New repository secret."

5. Add the following secrets:
   - `AWS_ACCESS_KEY_ID`: Your IAM role's AWS access key ID.
   - `AWS_SECRET_ACCESS_KEY`: Your IAM role's AWS secret access key.
   - `AWS_REGION`: The AWS region where your resources are located.
   - `AWS_DISTRIBUTION_ID`: The CloudFront distribution ID.

### 2. Create GitHub Actions Workflow

Create a `.github/workflows` directory in your repository if it doesn't exist. Then, create a new YAML file for your workflow, for instance, `deploy.yml`.

Copy and paste the following content into your `deploy.yml`:

```yaml
name: CI/CD for React Vite to S3 with CloudFront Invalidation

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: 14

      - name: Configure AWS Credentials
        run: |
          aws configure set aws_access_key_id ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws configure set aws_secret_access_key ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws configure set region ${{ secrets.AWS_REGION }}

      - name: Build React App
        run: |
          npm install
          npm run build 

      - name: Deploy app build to S3 bucket
        run: aws s3 sync ./dist s3://your-bucket-name/ --delete

      - name: Invalidate cache
        run: aws cloudfront create-invalidation --distribution-id ${{ secrets.AWS_DISTRIBUTION_ID }} --paths "/*"
