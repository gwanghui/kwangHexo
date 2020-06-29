---
title: jenkins-ssh-deploy
date: 2020-06-23 10:18:22
tags:
---
### Jenkins SSH Deploy
1. Jenkins Plug-in 설치
    - SSH Agent Plug-in : This plugin allows you to provide SSH credentials to builds via a ssh-agent in Jenkins.
2. jenkins Server에 jenkins 계정으로 public/private keypair 만들기
    - sudo -u jenkins /bash/sh
    - create a key pair
    ```text
       ssh-keygen
    ```
    - After entering the command, you should see the following prompt
    ```text
       Output
       Generating public/private rsa key pair.
       Enter file in which to save the key (/your_home/.ssh/id_rsa):
    ```
    - 생성하면 id_rsa, id_rsa.pub file이 만들어진다.
3. remote Server의 .ssh폴더에 authorized_keys 파일에 jenkins Server의 Jenkins 계정 public key 등록하기
4. Jenkins Credentials의 Global Credential에 jenkins 계정 private key 등록하기
5. pipeline 구성
    - pipeline
    ```text
       sshagent(['user_key_id']) {
           sh 'scp blur blur~'
           sh 'ssh -t -t userId@targetIp -o StrictHostKeyChecking=no "cd /usr/local && ./start.sh"'
       }
    ```

6. start.sh 생성
```shell script

```    
