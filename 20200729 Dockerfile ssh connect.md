# Training
2020/07/29 Dockerfile ssh connect

## Dockerfile
![Dockerfile](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/Dockerfile%200729.PNG)

```
FROM centos:7

RUN yum install openssh-server -y 

RUN /bin/echo "root" | passwd --stdin root

RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key \
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_ecdsa_key \
    && ssh-keygen -t rsa -f /etc/ssh/ssh_host_ed25519_key

RUN /bin/sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd \
    && /bin/sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config \
    && /bin/sed -i "s/#UsePrivilegeSeparation.*/UsePrivilegeSeparation no/g" /etc/ssh/sshd_config


EXPOSE 22

CMD ["/usr/sbin/sshd","-D"]
```

## build Dockerfile
![docker build Dockerfile](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20build%20dockerfile%200729.PNG)
## docker run image
![docker images](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20images%200729.PNG)
## docker run image & connect
![connect](https://github.com/Kevin-Lai-HPE/Training/blob/master/images/docker%20run%20sshdf7.PNG)
