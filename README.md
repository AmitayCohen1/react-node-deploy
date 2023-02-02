
# Deploy React & NodeJS to AWS

### 👋 Personal note
Please thoroughly review this process as things change and mistakes can be made.  
If you have any questions, feel free to reach out to amitay1599@gmail.com. 

###  Future updates to this document are planned to include
- Backup and disaster recovery plan 💾
- Continuous Integration/Continuous Deployment (CI/CD) 🤖
- Monitoring and performance tuning 📊
- Security best practices 🔒

Enjoy 🚀🚀


# Process

### AWS Services
- EC2 
- S3 
- CloudFront 
- Route 53 
- Elastic IPs (EC2)
- ACM (for CloudFront SSL)


### Other Services
- Nginx  
- CertBot (Nginx SSL) 
- PM2 
- GIT
- Domain provider



## Front
* Buy a domain.
* Connect it to Route 53 with Nameservers
* Upload React’s build folder to a new S3.
* Create a CloudFront distribution with an SSL certificate.

## Back
* Set up EC2 - Amazon Linux 2.
* Set up security group (https, ssh).
* Create a Route53 Record for connecting the IP address of the instance to api.mysite.com.
* Download Node, Git, and PM2.
* Connect GitHub SSH to EC2.
* Clone your repo.
* Add a testing route - app.get(‘/test’).
* Install dependencies & create .env file.
* Add CORS to node application.
* Run pm2 start server.js.
* Test route.

## Finish
* Download Nginx.
* Add proxy pass to nginx.conf.
* Test route.
* Update  nginx.conf for the new domain route (api.mysite.com).
* Set up SSL using CertBot.
* Add elastic IP for the instance assosiate it, and  change the record (api.mysite.com).
* Test everything.



## Commands


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



### Nginx

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





## 🔗 Links
###  SSL For Amazon linux 2 (start from “Install and run Certbot” section and change apache to nginx)  
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#letsencrypt


###  AWS Route 53 Domain Name 
https://www.youtube.com/watch?v=jDz4j_kkyLA 

###  Nginx Reverse Proxy on AWS EC2 Amazon Linux 2 
https://www.youtube.com/watch?v=_EBARqreeao

###  Deploy Node app on AWS EC2 Amazon Linux 2 
https://www.youtube.com/watch?v=oHAQ3TzUTro

###  Deploy React to CloudFront with HTTPS Custom Domain
https://www.youtube.com/watch?v=lPVgfSXTE1Y&t=1s

###  Setting Up And Deploying AWS EC2 Instances
https://www.youtube.com/watch?v=GEVbYQWWJkQ

