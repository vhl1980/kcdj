# BUILD IMAGE ALPINE JAVA 

I retrieve a docker file from below :
https://hub.docker.com/r/anapsix/alpine-java


``` bash
cd /home/ansible/project/kcdjr/docker/alpine-java
docker build -t anapsix/alpine-java:jdk-1.8.201 .

# TEST IMAGE
docker run -it --rm anapsix/alpine-java:jdk-1.8.201 java -version
```
