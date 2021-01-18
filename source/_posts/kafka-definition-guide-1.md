---
title: kafka-definition-guide-1
date: 2021-01-18 14:46:55
tags:
---
### Keyword
- page cache : 카프카는 프로듀서에게 빠른 응답 시간을 제공하기 위해 디스크 입출력 성능에 의존한다. !!
- dirty memory page
- swap space
- vm.swappiness : 이 매개변수의 값은 비율이며, 페이지 캐시의 페이지들을 삭제하지 않고 스와핑 공간을 얼마나 사용할지 나타낸다.
- linux file system
  - EXT4 
  - XFS
- File Metadata
  - ctime : 생성 시간
  - mtime : 마지막 수정 시간
  - atime : 마지막 사용 시간  
