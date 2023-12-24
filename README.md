# 2048 Game Docker Deployment

## Introduction

This repository contains a Dockerfile for deploying the popular 2048 game using Nginx on Ubuntu 22.04. The game source code is obtained from the [2048 GitHub repository](https://github.com/gabrielecirulli/2048/).

![Game Screenshot](game-ss.png)

## Game Source

The game source code is sourced from the repository: [gabrielecirulli/2048](https://github.com/gabrielecirulli/2048/).

## Dockerfile Explanation

### Dockerfile

```dockerfile
FROM ubuntu:22.04

RUN apt-get update
RUN apt-get install -y nginx zip curl

RUN echo "daemon off;" >> /etc/nginx/nginx.conf

RUN curl -o /var/www/html/master.zip -L https://github.com/gabrielecirulli/2048/archive/master.zip \
    && unzip -q /var/www/html/master.zip -d /var/www/html/ \
    && mv /var/www/html/2048-master/* /var/www/html/ \
    && rm -rf /var/www/html/2048-master /var/www/html/master.zip

EXPOSE 80

CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```

## Explanation

- **FROM ubuntu:22.04**: Sets the base image to Ubuntu 22.04.
- **RUN apt-get update**: Updates the package list.
- **RUN apt-get install -y nginx zip curl**: Installs Nginx, zip, and curl.
- **RUN echo "daemon off;" >> /etc/nginx/nginx.conf**: Configures Nginx to run in the foreground.
- **RUN curl -o /var/www/html/master.zip -L https://github.com/gabrielecirulli/2048/archive/master.zip \ && unzip -q /var/www/html/master.zip -d /var/www/html/ \ && mv /var/www/html/2048-master/ /var/www/html/ \ && rm -rf /var/www/html/2048-master /var/www/html/master.zip**: Downloads and extracts the 2048 game source code into the Nginx default directory.
- **EXPOSE 80**: Exposes port 80.
- **CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]**: Starts Nginx with the specified configuration file.

## Deployment to AWS Elastic Beanstalk

To deploy the container to AWS Elastic Beanstalk, follow these steps:

1. **Create an Elastic Beanstalk Environment**:
   - Log in to the AWS Management Console.
   - Navigate to Elastic Beanstalk.
   - Click on "Create Application" and follow the wizard to create an environment.

2. **Upload Docker Container**:
   - In the Elastic Beanstalk dashboard, click on your environment.
   - In the left sidebar, navigate to "Upload and Deploy" under "Deploy" section.
   - Upload your Docker container.

3. **Configure Environment Variables**:
   - Set any required environment variables.
   - Review and confirm the configuration.

4. **Deploy**:
   - Click "Deploy" to start the deployment process.

5. **Access Your Application**:
   - Once the deployment is successful, you can access your application using the provided URL.

Congratulations! Your 2048 game is now deployed on AWS Elastic Beanstalk.
