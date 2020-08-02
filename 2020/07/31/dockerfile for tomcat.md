# Training

## wget download jdk
https://javadl.oracle.com/webapps/download/GetFile/1.8.0_[xxx]-b[xx]/[encrypted_path]/linux-i586/[linux_tar_gz]

```wget https://javadl.oracle.com/webapps/download/GetFile/1.8.0_261-b12/a4634525489241b9a9e1aa73d9e118e6/linux-i586/jdk-8u261-linux-x64.tar.gz```
## wget download apache tomcat
```wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.37/bin/apache-tomcat-9.0.37.tar.gz```

```tar -xzvf jdk-8u261-linux-x64.tar.gz jdk1.8.0_261/```
```tar -xzvf apache-tomcat-9.0.37.tar.gz apache-tomcat-9.0.37/```

## vim Dockerfile
```
FROM centos:7
RUN yum update -y
RUN yum install -y httpd
RUN yum install -y net-tools
RUN yum install -y vim
RUN yum install -y telnet
 
RUN mkdir -p /traning/java/
ADD jdk1.8.0_261 /training/java/jdk1.8.0_261

RUN mkdir -p /training/java/apache-tomcat-9.0.37
ADD apache-tomcat-9.0.37 /training/java/apache-tomcat-9.0.37
 
ENV JAVA_HOME /training/java/jdk1.8.0_261
ENV CATALINA_HOME /training/java/apache-tomcat-9.0.37
ENV PATH $PATH:$JAVA_HOME/bin:CATALINA_HOME/bin

VOLUME ["/training/java/apache-tomcat-9.0.37/webapps"]

EXPOSE 8080

CMD ["/training/java/apache-tomcat-9.0.37/bin/catalina.sh", "run"]
```
## docker build

```docker build -t test .```

![docker build1](https://github.com/Kevin-Lai-HPE/Training/blob/master/2020/07/31/docker%20build%20-t%20test%201.PNG)
![docker build](https://github.com/Kevin-Lai-HPE/Training/blob/master/2020/07/31/docker%20build%20-t%20test%202.PNG)
## docker images
![docker images](https://github.com/Kevin-Lai-HPE/Training/blob/master/2020/07/31/docker%20images%20test.PNG)
## docker run
![docker run -d --name tomcat -p 8080:8080 test](https://github.com/Kevin-Lai-HPE/Training/blob/master/2020/07/31/docker%20run%20-d%20-p%208080%208080%20test.PNG)

```docker run -d --name tomcat -p 8080:8080 test```
## curl loaclhost:8080
![curl loaclhost:8080](https://github.com/Kevin-Lai-HPE/Training/blob/master/2020/07/31/curl%20localhost8080.PNG)
## find the directory of VOLUME

```docker inspect -f '{{.Mounts}}' tomcat```

### wget the sample file in {$VOLUME}

```wget  https://tomcat.apache.org/tomcat-9.0-doc/appdev/sample/sample(1,2).war```
