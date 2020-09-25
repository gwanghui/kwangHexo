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
    
### Response Header
- X-Content-Type-Option : 리소스를 다운로드할때 해당 리소스의 MIMETYPE이 일치하지 않는 경우 차단
    - 해당 헤더 추가
    - X-Content-Type-Options: nosniff
- Strict-Transport-Security : 한번 https로 접속하는 경우 이후의 모든 요청을 http로 요청하더라도 브라우저가 자동으로 https로 요청
    - 해당 헤더 추가
    - Strict-Transport-Security: max-age=63072000; includeSubdomains; preload
 - X-XSS-Protection : (XSS) 공격을 감지 할 때 페이지 로드를 중지시킬 수 있습니다.
    - 해당 헤더 추가
    - X-XSS-Protection: 1;mode=block
 - X-Frame-Options : 해당 페이지를 <frame> 또는 <iframe>, <object> 에서 렌더링할 수 있는지 여부를 나타내는데 사용됩니다.
    - 해당 헤더 추가
    - X-Frame-Options: sameorigin
- Referrer-Policy
    - 해당 헤더 추가
    - 'Referrer-Policy' 'origin';
```text
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1;mode=block";
add_header X-Frame-Options "sameorigin";
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
add_header 'Referrer-Policy' 'origin';
``` 

### Java
- 모든 중요한 쿠키에 'Secure' 속성을 더할 것
- The set-cookie was blocked because it has the secure 
```yaml
server:
  servlet:
    session:
      cookie:
        secure: true
```

### nginx CORS
```text
location / {
     if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Vary: Origin';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        #
        # Custom headers and headers various browsers *should* be OK with but aren't
        #
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        #
        # Tell client that this pre-flight info is valid for 20 days
        #
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain; charset=utf-8';
        add_header 'Content-Length' 0;
        return 204;
     }
     if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
     }
     if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range';
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';
     }
}
```
### On-Site Request Forgery Attack
### Cross-Site Request Forgery Attack
![](/images/project/security/access_control_allow.png)


