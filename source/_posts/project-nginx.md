---
title: project-nginx
date: 2020-06-25 08:53:07
tags:
---
### Nginx 설치
#### 사전 설치 필요 Library
- GCC (GNU) : GNU Compiler Collection
- PCRE (Perl Compatible Regular Expression) : URL 재작성 모듈 및 HTTP 핵심 모듈이 정규식을 사용한다.
- zlib : nginx gzip 압축 하는데 필요
- OPENSSL : SSL v2/v3, TLS v1 protocol 지원
 sudo apt-get install build-essential
 sudo apt-get install libpcre3 libpcre3-dev
 sudo apt install zlib zlib-devel
 sudo apt install zlib1g zlib1g-dev
 sudo apt install openssl libssl-dev
 
 sudo yum install gcc gcc-c++ make
 sudo yum install pcre pcre-devel
 sudo yum install zlib zlib-devel
 sudo yum install openssl openssl-devel

#### Make Compile
- ./configure --prefix=/home/lscns/project/nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_gzip_static_module
    - prefix = base nginx folder
    - make
    - make install 

#### Make Compile
- ./configure --prefix=/DATA/nginx --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_gzip_static_module
    - prefix = base nginx folder
    - make
    - make install 