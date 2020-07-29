# Training
2020/07/28 Docker build

## Pull image (Apache)
![docker pull httpd](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20pull%20httpd.PNG?raw=true)


## Dockerfile

```
FROM centos:7

RUN yum update -y

RUN yum install -y httpd

RUN yum install -y net-tools

RUN yum install -y vim

RUN yum install -y telnet
```

### build Dockerfile
![docker build Dockerfile](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20build%20dockerfile.PNG?raw=true)
### docker run image
![docker images](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20images.PNG?raw=true)
![docker run image](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20run%20image.PNG?raw=true)
### running container
![running container](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/success1.PNG?raw=true)
![curl apache](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/success.PNG?raw=true)
