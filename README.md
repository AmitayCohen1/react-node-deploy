
# Deploy React & NodeJS to AWS

### 👋 Personal note
Please thoroughly review this process as things change and mistakes can be made.  
If you have any questions, feel free to reach out to <amitay1599@gmail.com>

That's me https://amitaycohen.com/

###  Future updates to this document are planned to include
- Backup and disaster recovery plan 💾
- Continuous Integration/Continuous Deployment (CI/CD) 🤖
- Monitoring and performance tuning 📊
- Security best practices 🔒

Enjoy 🚀🚀


## Index

- [Process](#Process)
- [Useful Commands](#Useful-Commands)
- [🔗 Links](#Links)

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
* Connect to instance via SSH.
* Download Node, Git, and PM2.
* Connect GitHub SSH to EC2, and clone your repo.
* Add a testing route to your server (‘/test’).
* Install dependencies & create .env file if needed.
* Add CORS to node application based on your react origin.
* Run pm2 start server.js.
* Test route.


## Finish
* Download Nginx.
* Add proxy pass to nginx.conf.
* Test route.
* Update nginx.conf for the new domain route (api.mysite.com).
* Set up SSL using CertBot.
* Add elastic IP for the instance assosiate it, and  change the record (api.mysite.com).
* Test everything.



## Useful Commands


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

Useful Commands
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


[SSL For Amazon linux 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/SSL-on-amazon-linux-2.html#letsencrypt)
(from “Install and run Certbot” section and change apache to nginx)


[Route 53 - Domain  ](https://www.youtube.com/watch?v=jDz4j_kkyLA)

[Deploy Node on EC2](https://www.youtube.com/watch?v=_EBARqreeao)

[Deploy React to CloudFront with HTTPS Custom Domain](https://www.youtube.com/watch?v=lPVgfSXTE1Y&t=1s)

[Deploying EC2 Instances](https://www.youtube.com/watch?v=GEVbYQWWJkQ)



![myImage](https://media.giphy.com/media/XRB1uf2F9bGOA/giphy.gif)






