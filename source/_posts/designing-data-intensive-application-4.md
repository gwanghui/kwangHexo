---
title: designing-data-intensive-application-4
date: 2020-09-14 12:33:22
tags:
- data intensive
---

### Thrift
- Binary Protocol
![](/images/data-intensive/chapter_4/thrift_binary_protocol.png)

- Compact Protocol
![](/images/data-intensive/chapter_4/thrift_compact_protocol.png)
    - variable-length integer : 가변 길이 정수 부호화
        - 각 바이트의 상위 비트는 앞으로 더 많은 바이트가 있는지를 나타내는 데 사용한다.
        - -64 ~ 63 사이의 숫자는 1바이트로 부호화, -8192 ~ 8191 사이의 숫자는 2바이트로 부호화 한다는 의미
        