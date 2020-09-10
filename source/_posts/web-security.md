---
title: web-security
date: 2020-09-10 16:06:18
tags:
- web
- security
---
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

```text
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1;mode=block";
add_header X-Frame-Options "sameorigin";
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
```