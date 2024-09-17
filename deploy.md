
# Deploy React & NodeJS to AWS

### 👋 Personal note
This is a basic example of deploying an MVP to get up and running.      
If you have any questions, feel free to reach out to <amitay1599@gmail.com>

[That's me](https://www.amitaycohen.com/) 🧑🏼‍🚀 


###  Future updates to this document are planned to include
- Backup and disaster recovery plan 💾
- Continuous Integration/Continuous Deployment (CI/CD) 🤖
- Monitoring and performance tuning 📊
- Security best practices 🔒

Enjoy 🚀🚀

## Index

* [Process](#Process)
* [Useful Commands](#Useful-Commands)
* [Links](#-links)



# Process

### AWS Services
- EC2 - Virtual servers for running applications.
- S3 - Object storage for static files.
- CloudFront - Content delivery network for fast distribution of static files.
- Route 53 - Scalable domain name system for mapping domain names to IP addresses.
- Elastic IPs (EC2) - Static IP addresses for EC2 instances.
- ACM - SSL/TLS certificate management for secure communication over the internet.

### Other Services
- Nginx - Web server for serving dynamic content and managing traffic (our reverse proxy).
- CertBot - SSL certificate management tool for Nginx.
- PM2 - Process manager for Node.js applications.
- GIT.
- Domain provider.


## Front
* Buy a domain from a provider (GoDaddy, Google, etc).
* Connect it to Route 53 with Nameservers.
* Upload React’s build/dist folder to a new S3.
* Create a CloudFront distribution and request an SSL certificate.

## Back
* Lanuch a new EC2 instance - Amazon Linux 2.
* Set up security group - HTTPS, HTTP, SSH (after testing we will remove HTTP). 
* Create a Route 53 Record for connecting the IP address of the instance to api.mysite.com (Later we will switch it with an elastic ip).
* Connect to instance via SSH.
* Install Node, Git, and PM2.
* Connect GitHub SSH to your EC2, and clone your repo.
* Add a testing route to your server (‘/test’).
* Install dependencies & create .env file if needed.
* Add CORS to Node application based on your React origin.
* Run pm2 start server.js.
* Test route.

## Finish
* Install Nginx.
* Add proxy pass to nginx.conf.
* Update nginx.conf for the new domain route (api.mysite.com).
* Set up SSL using CertBot.
* Add elastic IP for the instance assosiate it, and  change the record (api.mysite.com).
* Go make some cool shit.


## Security Groups
* HTTP (port 80) - for serving web traffic over HTTP.
* HTTPS (port 443) - for serving web traffic over HTTPS.
* SSH (port 22) - for accessing the EC2 instance via SSH.



# Useful Commands


### NodeJS
Install (Amazon linux 2)

Check here: https://github.com/nodesource/distributions#ubuntu-versions

```bash
sudo yum install https://rpm.nodesource.com/pub_16.x/nodistro/repo/nodesource-release-nodistro-1.noarch.rpm -y
```

```bash
sudo yum install nodejs -y --setopt=nodesource-nodejs.module_hotfixes=1
```

```bash
sudo yum install gcc-c++ make
```



### Git
Install with yum

```bash
sudo yum install -y git
```



### Nginx

Install Nginx on Amazon linux 2 (2023)
```bash
sudo dnf install -y nginx
```

###  SSL


```bash
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip

sudo /opt/certbot/bin/pip install certbot certbot-nginx
sudo ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

```bash
sudo vim /etc/nginx/nginx.conf
```

In the HTTP block, add the following lines to redirect all HTTP traffic to HTTPS:


```bash
server {
listen 80;
server_name example.com;
return 301 https://$host$request_uri;
}
```

```bash
sudo certbot --nginx 
```

Commands
```bash
sudo systemctl start nginx
```

```bash
sudo systemctl restart nginx
```

```bash
sudo vim /etc/nginx/nginx.conf
```


Renew SSL (make sure to allow port 80)
```bash
sudo certbot renew
```



### pm2

Commands

```bash
sudo npm install pm2 -g
```

```bash
pm2 start server.js 
```

```bash
pm2 kill 
```

```bash
pm2 stop all
```

```bash
pm2 status
```

```bash
pm2 logs
```






### redis (2023)

Commands
```bash
sudo dnf install -y redis6 
```

```bash
sudo systemctl start redis6
```

```bash
sudo systemctl enable redis6
```

```bash
sudo systemctl is-enabled redis6
```

```bash
redis6-server --version
```

```bash
redis6-cli ping
```



example for ngnix conf: 
# User and process settings
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log notice;
pid /run/nginx.pid;

# Load dynamic modules
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    # Log format
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    # Performance settings
    sendfile            on;
    tcp_nopush          on;
    keepalive_timeout   65;
    types_hash_max_size 4096;

    # Include MIME types
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files
    include /etc/nginx/conf.d/*.conf;

    # SSL server block for the HTTPS API
    server {
        server_name api.bloodexplain.com;

        # Proxy settings to forward traffic to your Node.js app running on port 4000
        location / {
            proxy_pass http://localhost:4000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }

        listen 443 ssl; # managed by Certbot
        ssl_certificate /etc/letsencrypt/live/api.bloodexplain.com/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/api.bloodexplain.com/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
    }

    # HTTP server block for handling HTTP to HTTPS redirection
    server {
        listen 80;
        server_name api.bloodexplain.com;

        # Redirect HTTP requests to HTTPS
        return 301 https://$host$request_uri;
    }
}




# 🔗 Links


[SSL For Amazon linux 2](https://awswithatiq.com/ssl-setup-on-amazon-linux-2023-using-nginx-and-letsencrypt/)


[Route 53 - Domain  ](https://www.youtube.com/watch?v=jDz4j_kkyLA)

[Deploy Node on EC2](https://www.youtube.com/watch?v=_EBARqreeao)

[Deploy React to CloudFront with HTTPS Custom Domain](https://www.youtube.com/watch?v=lPVgfSXTE1Y&t=1s)

[Deploying EC2 Instances](https://www.youtube.com/watch?v=GEVbYQWWJkQ)

[Redis 2023 Amazon linux 2 EC2 Instace](https://serverfault.com/questions/1127483/how-to-install-and-configure-redis-server-on-amazon-linux-2023-al2023)




![myImage](https://media.giphy.com/media/XRB1uf2F9bGOA/giphy.gif)






