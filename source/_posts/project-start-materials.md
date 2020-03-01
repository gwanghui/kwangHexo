---
title: project-start-materials
date: 2020-03-01 20:26:59
tags:
---
## Jenkins

CI/CD 환경을 만들기 위해서 가장 익숙한 도구를 사용했다.
war파일을 받아 실행만 하면 매우 쉽기 때문에 설치해서 사용하려고 한다.

```shell script
#!/bin/bash

export HTTP_PORT=9090
export JENKINS_HOME=~/project/jenkins/jenkins_home

nohup java -jar jenkins.war --httpPort=$HTTP_PORT --sessionTimeout=120 -XX:+AggressiveOpts >> ./logs/jenkins.log 2>&1 &
```

아무 설정도 하지 않으면 JENKINS_HOME을 기본적으로 홈의 .jenkins 폴더에 구성하기 떄문에 Jenkins_home을 지정해주고,
Port 설정을 하지 않으면 8080 포트로 기동 되기에 HTTP_PORT 를 변경할 수 있도록 한다.
그리고 기동 후 $JENKINS_HOME/secrets/initialAdminPassword 를 물어보기 때문에 이를 넣고 기동하도록 하자

## Docker

Docker를 이용한 Container 배포를 하기 위해 로컬에서 Docker를 사용할 수 있도록 설치 해보자
https://docs.docker.com/install/linux/docker-ce/ubuntu/ 에 매우 잘 나와 있다.
### SET UP THE REPOSITORY

1. update the apt package index :
```shell script
apt update
```    

2. Install packages to allow apt to use a repository over HTTPS:
```shell script
sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg-agent \
     software-properties-common
```

3. Add Docker’s official GPG key:
GPG(PGP)는 암호화 프로그램으로 RSA 방식을 사용하며 주로 이메일을 암호화 하는데 사용된다.
 ```shell script
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88
```

4. Set up the stable repository
lsb_release -cs 는 Ubuntu distribution 정보를 알수 있다.
- Eoan 19.10
- Bionic 18.04 (LTS)
- Xenial 16.04 (LTS)

```shell script
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

### INSTALL DOCKER ENGINE - COMMUNITY

1. Update the apt package index.
```shell script
sudo apt update
```

2. Install the latest version of Docker Engine - Community and containerd, or go to the next step to install a specific version:
```shell script
sudo apt install docker-ce docker-ce-cli containerd.io
```   

3. Verify that Docker Engine
```shell script
sudo docker run hello-world
```

4. 확인
```shell script
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete 
Digest: sha256:fc6a51919cfeb2e6763f62b6d9e8815acbf7cd2e476ea353743570610737b752
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

