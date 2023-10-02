# CI/CD Setup for Server Deployment to EC2

This guide covers the process to set up Continuous Integration and Continuous Deployment (CI/CD) for our server using GitHub Actions, deploying directly to an AWS EC2 instance.

## Prerequisites

1. **GitHub Repository**: This guide assumes you have a GitHub repository where your server code is stored.
2. **EC2 Instance**: Ensure you have an EC2 instance running with the necessary environment for your application.
3. **SSH Access**: Ensure your EC2 instance is set up to allow SSH access.

## Steps

### 1. Setup Secrets in GitHub

Your GitHub repository uses secrets to store sensitive information that your GitHub Actions will use. Here's how to set them up:

1. Navigate to your GitHub repository.
2. Click on the "Settings" tab.
3. Under "Settings", click on "Secrets".
4. Click on "New repository secret".
5. Add the following secrets:
   - `EC2_HOST`: Your EC2 instance's public IP address or hostname.
   - `EC2_USER`: Typically, this is `ec2-user` for Amazon Linux instances.
   - `EC2_PEM`: Your EC2 private key content. Copy everything from `-----BEGIN RSA PRIVATE KEY-----` to `-----END RSA PRIVATE KEY-----` including the headers.

### 2. Setup GitHub Actions YAML

Create a `.github/workflows` directory in your repository if it doesn't exist. Then, create a new YAML file for your workflow, for instance, `deploy.yml`.

Copy and paste the following content into your `deploy.yml`:

```yaml
name: Deploy to EC2
on: [push]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Deploy to EC2
      uses: appleboy/ssh-action@master
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USER }}
        key: ${{ secrets.EC2_PEM }}
        script: cd /home/ec2-user/readablle-back && git pull && npm install && pm2 restart all

