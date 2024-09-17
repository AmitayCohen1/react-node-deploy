# CICD

### **Step-by-Step Guide**


# POTENTIONAL ISSUES: 
 - SECUTIRY GROUPS 
- MAKE SURE .GITIGNORE HAS .ENV SO PORT="" WONT MESS UP WITH THINGS
- DONT USE NGNIX UP UNTILL YOU FINISH THAT 



**1. Setting Up Your AWS EC2 Instance:**

- **Launch EC2 Instance:**
    - Log in to the AWS Management Console and launch an EC2 instance with your preferred settings.
    - Ensure that the security group associated with your EC2 instance allows SSH access (usually on port 22).
- **Configure Your EC2 Instance:**
    - Connect to your EC2 instance using SSH.
    - Install Node.js, npm, and any other dependencies required for your application.
    - Install **`git`** if you plan to pull the latest code from a repository.
    - Install PM2 or another process manager to keep your Node.js application running.

**2. Generate and Add Your SSH Key:**

- **Generate an SSH Key Pair on Your Local Machine:**
    - Open a terminal.
    - Run the following command to generate a new SSH key pair:Replace **`"your_email@example.com"`** with your email address. This comment helps identify the key.
        
        ```bash
        ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
        ```
        
    - Follow the prompts to save it to a file within the **`.ssh`** directory (e.g., **`/home/youruser/.ssh/id_rsa`**).
    - You will get two files, for example **`id_rsa`** and **`id_rsa.pub`**. The **`.pub`** file is your public key.
- **Add Your Public Key to EC2:**
    - Copy the content of your public key file (ca) by displaying it using the command **`cat ~/.ssh/id_rsa.pub`** and copying the output.
    - SSH into your EC2 instance.
    - Append the public key content into **`~/.ssh/authorized_keys`** for the user you will use for SSH in your EC2.
    - 
    
    ```bash
    echo 'your-public-key-text' >> ~/.ssh/authorized_keys
    ```
    
    - Set the correct permissions for the **`.ssh`** directory and the **`authorized_keys`** file with the following commands on your EC2 instance:
        
        ```bash
        chmod 700 ~/.ssh
        chmod 600 ~/.ssh/authorized_keys
        ```
        

**3. Set Up Repository Secrets in GitHub:**

- **GitHub Secrets:**
    - Navigate to your GitHub repository, click on 'Settings' > 'Secrets' > 'New repository secret'.
    - Add the following secrets:
        - **`EC2_HOST`**: The public IP address or DNS name of your EC2 instance.
        - **`EC2_USER`**: The username for SSH on your EC2 instance (e.g., **`ec2-user`**).
        - **`SSH_PRIVATE_KEY`**: The entire content of your private SSH key (**`id_rsa`**). Ensure you copy the entire content of the private key, including the **`BEGIN`** and **`END`** lines.
    
    ```bash
    cat ~/.ssh/id_rsa
    ```
    

**4. Create the GitHub Actions Workflow:**

- **Workflow File:**
    - In your repository, create a directory named **`.github/workflows`**.
    - Inside this directory, create a file named **`deploy.yml`**.
    - Add the following content to **`deploy.yml`**:

```yaml
name: Deploy to EC2

on:
  push:

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
        key: ${{ secrets.SSH_PRIVATE_KEY }}
        script: |
          cd /path/to/your/project
          git pull
          npm install --production
          pm2 restart all
```

**5. Test the GitHub Actions Workflow:**

- **Push and Check:**
    - Commit and push the workflow file to your GitHub repository.
    - Push a change to your repository to trigger the workflow.
    - Check the 'Actions' tab in your GitHub repository to see the workflow run.

**6. Verify Deployment:**

- **SSH to EC2 Instance:**
    - After the workflow completes, SSH into your EC2 instance to verify that the application has been updated and restarted by PM2.

By following these instructions, you should be able to automatically deploy your code to an EC2 instance using GitHub Actions upon every push to your repository.
