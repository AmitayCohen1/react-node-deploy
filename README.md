
# Deploy React & NodeJS to AWS

### 👋 Personal note
Please thoroughly review this process as things change and mistakes can be made.  
This is a basic example of deploying an MVP to get up and running, there are better architectures out there.
I'm no expert. This is a general summary, more extensive documents to be available soon.
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
* Delete HTTP from your security group.
* Add elastic IP for the instance assosiate it, and  change the record (api.mysite.com).
* Go make some cool shit.



# Useful Commands


### NodeJS
Install (Amazon linux 2)

```bash
$ sudo yum install -y gcc-c++ make
```

```bash
$ curl -sL https://rpm.nodesource.com/setup_16.x | sudo -E bash -
```

```bash
$ sudo yum install -y nodejs
```


### Git
Install with yum

```bash
$ sudo yum install -y git
```



### Nginx

Install (Amazon linux 2)
```bash
$ sudo amazon-linux-extras install epel -y 
```

```bash
$ sudo yum install -y certbot python2-certbot-nginx
```

```bash
$ sudo certbot
```

Commands
```bash
$ sudo systemctl start nginx
```

```bash
$ sudo systemctl restart nginx
```

```bash
$ sudo vim /etc/nginx/nginx.conf
```



### pm2

Commands
```bash
$ pm2 start server.js 
```

```bash
$ pm2 kill 
```

```bash
$ pm2 stop all
```

```bash
$ pm2 status
```

```bash
$ pm2 logs
```


# 🔗 Links


[SSL For Amazon linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#letsencrypt)
(from “Install and run Certbot” section and change apache to nginx)


[Route 53 - Domain  ](https://www.youtube.com/watch?v=jDz4j_kkyLA)

[Deploy Node on EC2](https://www.youtube.com/watch?v=_EBARqreeao)

[Deploy React to CloudFront with HTTPS Custom Domain](https://www.youtube.com/watch?v=lPVgfSXTE1Y&t=1s)

[Deploying EC2 Instances](https://www.youtube.com/watch?v=GEVbYQWWJkQ)



![myImage](https://media.giphy.com/media/XRB1uf2F9bGOA/giphy.gif)






