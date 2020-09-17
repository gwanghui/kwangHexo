---
title: designing-data-intensive-application-5
date: 2020-09-14 15:24:24
tags:
- shared data
- Replication
---

## Replication
- Single Leader Replication
- Multi Leader Replication
- LeaderLess Replication

### Leader & Follower
- Replica : 복제 서버
    - 모든 복제 서버에 모든 데이터가 있다는 사실을 어떻게 보장하는가?
        - Leader-Based replication (active/passive, master/slave) : 복제 서버 중 하나를 리더로 지정하고, 리더는 로컬 저장소에 새로운 데이터를 기록
          복제 서버는 팔로워(follower)라고 한다. 리더가 로컬 저장소에 새로운 데이터를 기록할 때마다 데이터 변경을 Replication Log or Change Stream의 일부로 팔로워에게 전송
          각 팔로워가 리더로부터 로그를 받으면 리더가 처리한 것과 동일한 순서로 모든 쓰기를 적용해 복사본을 갱신한다.
          ![](/images/data-intensive/chapter_5/replication_copy.png)
    
- 동기식 대 비동기식 복제
    - 동기식 : 팔로워가 리더와 일관성 있게 최신 복사본을 가지는 것을 보장, Block 때문에 처리 대기 시간 발생
        - 현실적으로 데이터베이스에서 동기식 복제를 사용하려면 보통 팔로워 하나는 동기식으로 하고 그 밖에는 비동기식으로 하는 것을 의미
        - 적어도 두 노드에 데이터의 최신 복사본이 있는 것을 보장한다. 이것을 semi-synchronous(반동기식)이라 한다.
    - 비동기식 : 리더 기반 복제는 완전히 비동기식으로 구성한다. 리더가 잘못되고 복구할 수 없으면 팔로워에 아직 복제되지 않은 모든 쓰기는 유실된다.
    
### Follower failover
- 각 팔로워는 리더로부터 수신한 데이터 변경 로그를 로컬 디스크에 보관
- 팔로워는 리더에 연결해 팔로워 연결이 끊어진 동안 발생한 데이터 변경을 모두 요청

### Leader failover
- 팔로워 중 하나를 새로운 리더로 승격 & 다른 팔로워는 새로운 리더로부터 데이터 변경을 읽어오기 시작해야한다.
- 자동 장애 복구 
    - 리더 장애 판단 : 보통 타임아웃으로 체크
    - 새로운 리더 선택 : 보통 이전 리더의 최신 데이터 변경사항을 가진 Follower중에서 선출
    - 새로운 리더 사용을 위해 시스템을 재설정
- 자동 장애 복구 시 실패 Case
    - 비동기식 복제를 사용하면 새로운 리더는 이전 리더가 실패하기 전 이전 리더의 쓰기를 일부 수신하지 못할 수 있다.
    - Out of date Follower Leader 선출 문제
    - 특정 결함 시나리오 (Split Brain : 두 노드가 모두 자신이 리더라고 믿을 수 있다.)
    - 리더가 분명히 죽었다고 판단 가능한 적절한 타임아웃은 얼마인가?

    
  
        