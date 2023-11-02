# **DRAFT** - CI/CD Setup for Server Deployment to EC2

This guide covers setting up Continuous Integration and Continuous Deployment (CI/CD) for a server using GitHub Actions, deploying directly to an AWS EC2 instance.

## Prerequisites

1. **GitHub Repository**: A GitHub repository where your server code is stored.
2. **EC2 Instance**: An EC2 instance running with the necessary environment for your application.
3. **SSH Access**: Your EC2 instance should allow SSH access.

## Steps

### 1. Setup Secrets in GitHub

Store sensitive information securely in your GitHub repository's secrets for use by GitHub Actions.

- Navigate to your repository, then to Settings > Secrets.
- Add the following secrets:
  - `EC2_HOST`: The public IP address or hostname of your EC2 instance.
  - `EC2_USER`: Typically `ec2-user` for Amazon Linux instances.
  - `EC2_PEM`: The private key for SSH, including everything from `-----BEGIN RSA PRIVATE KEY-----` to `-----END RSA PRIVATE KEY-----`.

## Using RSA SSH Key for CI/CD on macOS

### Generating RSA SSH Key Pair

1. **Generate an SSH Key Pair**

```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/my-cicd-key
```

Replace "your_email@example.com" with your email address. This will create my-cicd-key (private key) and my-cicd-key.pub (public key) in the ~/.ssh directory.

1. Secure the Private Key:

Ensure the generated private key is kept secure:

```bash
chmod 400 ~/.ssh/my-cicd-key
```

### Add Public Key to EC2
   Append the contents of my-cicd-key.pub from your ~/.ssh directory to the ~/.ssh/authorized_keys file on your EC2 instance. This allows the holder of the private key to SSH into the instance.
   Using with GitHub Actions
   Encode the Private Key for GitHub:
   Convert the private key to a Base64 string to store in GitHub Secrets. On macOS, use:

```bash
base64 < ~/.ssh/my-cicd-key | tr -d '\n'
```

Copy the output string.

## Store in GitHub Secrets

Navigate to your GitHub repository's settings, go to 'Secrets', and add a new secret containing the Base64-encoded string.

## Security Considerations

1. Keep the Private Key Secret:
   Only share the public key. The private key should be kept confidential.

2. Regularly Rotate Keys:
   Periodically regenerate and rotate your SSH keys.

3. Restrict Access:
   Ensure only the necessary personnel and processes have access to the private key.

### 2. Setup GitHub Actions Workflow

Create a CI/CD workflow in your repository.

- Create a `.github/workflows` directory if not present.
- Add a new file, `deploy.yml`, with the following content:

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
          script: |
            cd /home/ec2-user/your-project-directory
            git pull
            npm install
            pm2 restart all
```

Replace `/home/ec2-user/your-project-directory` with your project's path on the EC2 instance.

### 3. Commit and Push

- Commit the `deploy.yml` file to your repository.
- Push it to the branch you want to trigger deployment from (e.g., `main`).
- This workflow will execute automatically when changes are pushed.

## Conclusion

This setup provides a basic CI/CD pipeline for deploying your application to an AWS EC2 instance using GitHub Actions.
