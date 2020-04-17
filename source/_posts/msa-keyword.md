---
title: msa-keyword
date: 2020-04-16 18:53:50
tags:
- keyword
categories:
- MicroServiceArchitecture
---
### Bus Factor
- 팀원들 사이에 공유디ㅗ지 않은 지식의 위험에 대한 척도.
- '만약 버스에 치이면'이라면 표현에서 비롯된 것으로 Truck Factor로도 알려져 있다. 버스 팩터가 낮을수록 더 나쁘다.

### 운영 준비 상태
- 신뢰성 
    - 서비스가 가용하고 에러에 대응 할 수 있는가? 
    - 배포 프로세스가 새로운 기능을 배포할 때 불안정성과 결함을 유발하지 않을 것을 신뢰할 수 있는가?
- 확장성
    - 서비스가 필요로 하는 리소스와 용량을 이해하는가?
    - 부하 상황에서 어떻게 서비스의 응답성을 유지할 것인가?
- 투명성
    - 로그와 메트릭을 통해 서비스의 운영을 관찰할 수 있는가?
    - 뭔가 잘못되면 누군가 알림을 받는가?
- 장애 내성
    - 단일 장애 지점을 어떻게 극복할 것인가?
    - 의존하는 다른 서비스의 장애를 어떻게 대응할 것인가?

### MSA 초기 단계 기본 원칙
- 품질 관리 : 코드 변경을 리뷰하고, 적절한 테스트를 작성하고 소스 코드의 버전 제어를 관리한다.
- 자동 배포 : 코드 변경을 운영으로 전달하는 것을 완전하게 검증하고 엔지니어의 개입을 최소화 해야한다.
- 회복성 : 서비스가 어떻게 실패할 수 있고 능동적으로 어떻게 사전에 대책을 강구할 수 있는지를 고려해야 한다.
- 투명성 : 서비스의 상태와 행동은 관측 가능해야 한다.

### 콘웨이의 법칙(Conway's law)
- 소프트웨어 구조는 해당 소프트웨어를 개발한 조직의 커뮤니케이션 구조를 닮게 된다.
https://johngrib.github.io/wiki/Conway-s-law/

### 아키텍처 원칙
- 개발 실무는 외부 표준을 준수해야 한다. (ISO 27001)
- 모든 데이터는 이동 가능해야 하고 보관 기간을 제한하는 것을 염두에 둬야 한다.
- 개인 정보는 어플리케이션을 통해 명확하게 추적할 수 있어야 한다.

### Bounded Context
- 컨텍스트 내의 모델은 응집도가 높고 실 세계와 동일한 뷰를 가진다
- 명확한 범위와 경계를 가지는 응집된 단위

### OpenAPI Specification 
- https://github.com/OAI/OpenAPI-Specification
- Community-driven open specification within the OpenAPI Initiative (https://www.openapis.org/)

### YANGI
- You Aren't Gonna Need It.

### DRY
- don`t repeat yourself. 중복된 반복 작업

### CAP 
- Consistency (일관성)
- Availability (가용성)
- Partition Tolerance (파티션 내성)
- CAP Twelve Years Later : How the 'Rules' have changed / Http://mng.bz/HGA3 
- http://ksat.me/a-plain-english-introduction-to-cap-theorem

### CQRS

### choreographed
- 자율적으로 구성된

### SAGA pattern
- Life Beyond Distributed Transactions http://queue.acm.org/detail/cfm?id=3025012


#### Choreography SAGA Pattern
#### Orchestration SAGA Pattern (조율된 사가 패턴)
- netflix conductor
#### Interwoven SAGA Pattern (중첩된 사가 패턴)
- 회로 차단하기 (short-circuiting)
- 잠그기 (Locking)
- 인터럽트 (Interruption)

### 일관성 패턴
1. 보상 동작 : 이전 동작을 없던 일로 하는 동작을 수행한다.
2. 재시도 : 성공 또는 시간 만료될 때가지 재시도한다.
3. 무시 : 에러 이벤트가 발생해도 아무것도 하지 않는다.
4. 재시작 : 원래 상태로 초기화하고 다시 시작한다.
5. 잠정적 동작 : 잠정적 동작을 수행하고 나중에 확정 또는 취소한다.

### 이벤트 소싱 패턴
- Nick Chamberlain : awesome-ddd (https://github.com/heynickc/awesome-ddd)

### Cache Problem
- https://markheath.net/post/troublehooting-caching-problems
 
### Idempotent
- 멱등성 : 연산을 여러번 적용 하더라도 결과가 달라지지 않는 성질

### 신뢰할 수 있는 커뮤니케이션 설계하기
- 재시도 : 기하급수적 백-오프(exponential back-off)
    - 항상 최대 재시도 횟수를 제한한다.
    - 기하급수적 백-오프와 지터(무작위 요소; jitter)를 포함해서 재시도 요청을 부드럽게 분산시키고 부하가 몰리는 것을 회피한다.
    - 어떤 에러 조건에서 재시도를 해야 할지, 그래서 어떤 재시도가 실패할 것 같은지를 고려한다.
- 폴백
    - 우아한 서비스의 저하(graceful degradation)
    - 캐싱 (Caching)
    - 기능 중복 (Functional redundancy)
    - 대체 데이터 (Stubbed data)
- 타임 아웃
- 회로 차단기 (서킷 브레이커)
    - half open    

