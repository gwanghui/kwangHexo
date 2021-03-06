---
title: designing-data-intensive-application_1
date: 2020-09-07 07:52:27
tags:
- data intensive
---
# Data intensive application
- Database : 구동 어플리케이션이나 다른 어플리케이션에서 나중에 다시 데이터를 찾을 수 있게 데이터를 저장
- Cache : 읽기 속도 향상을 위해 값비싼 수행 결과를 기억
- Search Index : 사용자가 키워드로 데이터를 검색하거나 다양한 방법으로 필터링 할 수 있게 제공
- Stream Processing : 비동기 처리를 위해 다른 프로세스로 메시지 보내기
- Batch Processing : 주기적으로 대량의 누적된 데이터를 분석
 
 ![](/images/data-intensive/chapter_1/data_system_architecture.png)
 
## 3가지 관심사
 - Reliability : 하드웨어나 소프트웨어 결함 심지어 Human Error 같은 경우에도 시스템은 올바르게(원하는 성능 수준에서 정확한 기능) 동작해야한다.
 - Scalability : 시스템 데이터의 양, 트래픽 양, 복잡도가 증가하면서 이를 처리 가능해야한다.
 - Maintainability: 시간이 지남에 따라 여러 다양한 사람들이 시스템 상에서 작업할 것이기 때문에 모든 사용자가 시스템 상에서 생산적으로 작업할 수 있어야 한다.
 
### 신뢰성 (Reliability)
- 어플리케이션은 사용자가 기대한 기능을 수행한다.
- 시스템은 사용자가 범한 실수나 예상치 못한 소프트웨어 사용법을 허용할 수 있다.
- 시스템 성능은 예상된 부하와 데이터 양에서 필수적인 사용 사례를 충분히 만족한다.
- 시스템은 허가되지 않은 접근과 오남용을 방지한다.
- 잘못될 수 있는 일을 결함(Fault)라고 부른다. 또한 이를 예측하고 대처할 수 있는 시스템을 내결함성(Fault-tolerant) or 탄력성 (resilent)을 지녔다고 말한다.
    - 내결함성은 오해의 소지를 가지고 있는데 모든 결함을 견디는 것이 아니라 특정 유형의 결함 내성에 대해서만 이야기하는 것이 타당하다.
- 결함(Fault)은 장애(Failure)와 동일하지 않다.
    - 장애는 사용자에게 필요한 서비스를 제공하지 못하고 시스템 전체가 멈춘 경우
    - 결함 확률을 0으로 줄이는 것은 불가능하다.

#### 하드웨어 결함
- 대규모 정전, 하드디스크 고장, 램 결함, 네트워크 케이블 결함 등
- 하드디스크 : RAID구성, 서버 : Hot Swap CPU, 데이터 센터 : 건전지, 예비 전원용 디젤 발전기

#### 소프트웨어 오류
- Systemic error(시스템 내 체계적 오류)
    - 리눅스 커널 버그
    - CPU 시간, 메모리, 디스크 공간, 네트워크 대역폭 처럼 공유 자원을 과도하게 사용하는 일부 프로세스
    - 시스템의 속도가 느려져 반응이 없거나 잘못된 응답을 반환하는 서비스
    - 한 구성 요소의 작은 결함이 다른 구성요소의 결함을 야기해 더 많은 결함이 발생하는 연쇄장애(Cascading failure)

### 확장성 (Scalability)
- 시스템이 특정 방식으로 커지면 이에 대처하기 위한 선택은 무엇인가?
- 추가 부하를 다루기 위해 계산 자원을 어떻게 투입할까?

#### 부하 기술하기
- Load Parameter
    - 웹 서버의 초당 요청 수
    - 데이터베이스의 읽기 대 쓰기 비율
    - 대화방의 동시 활성 사용자 수 (Active User)
    - 캐시 적중률
- EX) Twitter
    - 주요 기능
        - 트윗(tweet) 작성 : 사용자는 팔로워에게 새로운 메세지를 게시할 수 있다. (평균 초당 4.6k 요청, 피크일 때 초당 12k 요청 이상)
        - 홈 타임라인(timeline) : 사용자는 팔로우한 사람이 작성한 트윗을 볼 수 있다. (초당 300k 요청)
    - 트위터의 확장성 문제는 Fan-out 문제이다.
        - fan-out : 전자공학에서 빌려온 용어로, 다른 게이트의 출력에 배속된 논리 게이트의 입력의 수를 뜻한다. 출력은 배속된 모든 입력을 구동하기 위한 충분한 전류를 공급해야 한다.
                    트랜잭션 처리 시스템에서 하나의 수신 요청을 처리하는 데 필요한 다른 서비스의 요청 수를 설명하기 위해 팬 아웃을 사용한다.
- 부하 매개변수를 증가시키고 시스템 자원을 변경하지 않고 유지하면 시스템 성능은 어떻게 영향을 받을까?
- 부하 매개변수를 증가시켰을 때 성능이 변하지 않고 유지되길 원한다면 자원을 얼마나 많이 늘려야 할까?

#### 시스템 성능
- Throughput : 초당 처리할 수 있는 레코드 수나 일정 크기 데이터 집합으로 작업을 수행할 때 걸리는 전체 시간 / ex) Hadoop
- Response Time: 응답 시간, 클라이언트가 요청을 보내고 응답을 받는 사이의 시간
- Latency vs Response Time : 응답시간은 클라이언트 관점, 지연시간은 요청이 처리되길 기다리는 시간으로 서비스를 기다리며 휴지 상태인 시간을 말한다.
- 평균 응답 시간: 산술 평균(Arithmetic Mean)보다는 백분위, Median(중앙)값
- 상위 백분위 : 95분위, 99분위, 99.9 분위 (P95, P99, P999)
- Tail latency(꼬리 지연 시간) : 상위 백분위 응답시간
- 서비스 수준 목표(Service Level Object: SLO), 서비스 수준 협약서 (Service Level Agreement: SLA)
    - 큐 대기 지연(queueing delay) : 높은 백분위에서 응답시간 상당 부분을 차지한다. ==>  head of line blocking (선두 차단)
- 실전 백분위
    - tail latency amplification
    - 1분마다 구간 내 중앙값과 다양한 백분위를 계산에 각 지표를 그래프에 그리면 된다.
    - 시간 구간내 모든 요청의 응답시간 목록을 유지하고 1분마다 목록을 정렬하는 방법이 있다.
    - Forward Decay, T-digest, Hdr histogram 
 ![](/images/data-intensive/chapter_1/slow_application_effect.png)

#### 부하 대응 접근 방식
- 비공유(shared-nothing) 아키텍처 : 다수의 장비에 부하를 분산하는 아키텍처
    - Scaling up (수직 확장) 
    - Scaling out(수평 확장) 
- 아키텍처를 결정하는 요소 : 읽기의 양, 쓰기의 양, 저장할 데이터의 양, 데이터의 복잡도, 응답 시간 요구사항, 접근 패턴

#### 유지보수성
- 버그 수정, 시스템 운영 유지, 장애 조사, 새로운 플랫폼 적응, 기술 채무 상환, 새로운 기능 추가
- 운용성 : 운영의 편리함 만들기
    - 시스템 상태를 모니터링하고 상태가 좋지 않다먼 빠르게 서비스를 복원
    - 시스템 장애, 성능 저하 등의 문제 원인 추적 
    - 보안 때치를 포훌빼 소프트웨어와 플랫폼을 최신 상태로 유지
    - 다른 시스템이 서로 어떻게 영향을 주는 지 확인해 문제가 생길수 있는 변경사항을 손상을 입히기 전에 차단 
    - 미래에 발생 가능한 문제를 예측해 문제가 발생하기 전에 해결 (예를 들어 용량 계획)
    - 배포설정관리등을위한모범A떼와도구를마련
    - 어플리케이션을 특정 플랫폼에서 다른 플랫폼으로 이동하는 등 복응한 유지보수 태스크를 수행 
    - 설정 변경으로 생기는 시스템 보안 유지보수
    - 예측 가능한 운영과 안정적인 서비스 환경을 유지하기 위한 절차 정의
    - 개인 인사 이동에도 시스템에 대한 조직의 지식을 보존함
- 데이터 시스템 개선
    - 좋은 모니터링으로 런타임(runtime) 동작과 시스템의 내부에 대한 가시성 제공
    - 표준 도구를 이용해 자동화와 통합을 위한 우수한 자원을 제공
    - 개별 장비 의존성을 회피, 유지보수를 위해 장비를 내리더라도 시스템 전체에 영향을 주지 않고 계속해서 운영 가능해야 함
    - 좋은 문서와 이해하기 쉬운 운영 모댈(예를 들어 “X를 하면 Y가 발생한다") 제공
    - 만족할 만한 기본동작을 제공하고, 필요할 때 기본값을 다시 정의할 수 있는 자유를 관리자에게 부여
    - 적절하게 자기 회복{self--healing)이 가능할 뿐 아니라 필요에 따라 관리자가 시스템 상태를 수동으로 제어할 수 있게 힘
    - 예측 가능하게 동작하고 예기치 않은 상황을 최소화
- 단순성 : 복잡도 관리
    - 추상화
- 발전성 : 변화를 쉽게 만들기
    - TDD, Refactoring  