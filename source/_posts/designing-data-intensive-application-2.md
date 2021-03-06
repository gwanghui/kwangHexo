---
title: designing-data-intensive-application_2
date: 2020-09-07 08:40:01
tags:
- data intensive
---
# 데이터 모델과 질의 언어
- 데이터 모델을 표현하는 방법
    - 애를리케이션 개발자는 현실(사람， 조직， 상품， 행동， 자금 흐름 센서 등)을 보고 객체나 데이터 구조 그리고 이러한 데이터 구조를 다루는 API를 모델링한다 이런 구조는 보통 애플리케이션에 특화돼 있다.
    - 데이터 구조를 저장할 때는 JSON나 XML 문서， 관계형 데이터베이스 테이블이나 그래프 모댈 같은 범용 데이터 모델로 표현한다.
    - 더 낮은 수준에서 하드웨어 엔지니어는 전류， 빛의 따동， 자기장등의 관점에서 바이트를 표현하는 방법을 일이냈다.
    
## 데이터 베이스를 강력하게 만드는 데이터 구조
### Index
- Index의 일반적인 개념은 부가적인 메타데이터를 유지하는 것
- 많은 데이터베이스는 색인의 추가와 삭제를 허용
- 추가적인 구조의 유지보수는 특히 쓰기 과정에서 오버헤드 발생
- 쓰기의 경우 단순히 파일에 추가할 때의 성능을 앞서기 어렵다. Index를 유지하는 경우 매번 데이터를 쓸 때마다 갱신해주어야 하므로 쓰기속도를 느리게 만든다. 
### Hash Index
- Dictionary Type(사전 타입)
    - 보통 Hash Map이나 HashTable로 구
    - In-Memory HashMap으로 구성시 고성능 읽기 쓰기를 보장
    - 항상 추가만 한다면 사용 가능한 공간이 부족해지고 특정 크기의 세그먼트(Segment)로 로그를 나누는 방식
![](/images/data-intensive/chapter_2/segment_compaction.png)

bitcask : https://github.com/basho/bitcask
- 구현시 중요한 문제들
    - 파일 형식
    - 레코드 삭제 : 보통 로그 세그먼트가 병합될 때 툼스톤이라고 불리는 것을 추가한다.
    - 고장(Crash) 복구 : 각 세그먼트 해시 맵을 메모리로 빠르게 로딩하기 위해 snapshot 활용 및 다른 추가사항 고려
    - 부분적 레코드 쓰기 : 체크섬 포함해서 로그 손상된 부분 탐지
    - 동시성 제어 : 쓰기 쓰레드는 1개, 읽기 쓰레드는 여러개
    - 정해진 자리에 파일을 갱신하는 방법
        - 무작위 쓰기보다 순차적인 쓰기 방법이 더 빠르다
        - 세그먼트 파일이 추가 전용이나 불변이면 동시성과 고장 복구는 훨씬 간단하다.
        - 조각화되는 데이터 파일 문제를 피할 수 있다.
    - 해시테이블의 한계
        - 메모리에 저장해야 하므로 키가 너무 많으면 문제가 된다. 디스크에 해시맵을 유지하면 성능상 이점이 없어진다. 
        - 범위 질의에는 효율적이지 않다   

### SST(Sorted String Table) & LSM TREE
- Example : Cassandra(SST, LSM Tree, Bloom Filter)
- 일련의 키-쌍 값을 키로 정렬하는 것이다.
- 키로 정렬된 형식을 SST라고 한다.
- 세그먼트 병합은 입력파일을 함께 읽고 각 파일의 첫 번째 키를 본다. 그리고 가장 낮은 키를 출력 파일로 복사한 뒤 이 과정을 반복한다. 
  다중 세그먼트에 동일한 키가 있을 경우 가장 최근 세그먼트의 값은 유지하고 오래된 세그먼트의 값은 버린다. 
- 파일에서 특정 키를 찾기 위해 더는 메모리에 모든 키의 색인을 유지할 필요가 없다.
- 정렬되어 있는 키의 집합을 가지고 있기 때문에 해당 집합에서 찾지 못할 경우 가장 근접한 키값부터 Scan해서 찾는다.
- 읽기 요청은 요청 범위 내에서 여러 키-값 쌍을 스캔해야 하기 때문에 해당 레코드들을 블록으로 그룹화하고 디스크에 쓰기전에 압축한다. 

#### SST 생성과 유지
- 쓰기가 들어오면 인메모리 균형 트리(Balanced Tree) 데이터 구조에 추가한다. (memtable)
- 멤테이블이 보통 수 메가바이트 정도의 임계값보다 커지면 SS테이블 파일로 디스크에 기록한다. 트리가 이미 키로 정렬된 키-쌍값을 유지하고 있어 효율적으로 수행 가능
- 읽기 요청이 오면 멤테이블에서 키를 찾고, 그 다음 디스크상의 세그먼트들에서 찾는다.
- 가끔 세그먼트 파일을 합치고 덮어 쓰여지거나 삭제된 값을 버리는 병합과 컴팩션 과정을 수행한다. (Background)
- 데이터 베이스가 고장나면 멤테이블에 있는 가장 최신쓰기는 손실되어 분리된 로그를 디스크 상에 유지해야한다.

### LSM 트리 만들기
- 로그 구조화 병합 트리(Log-Structed Merge-Tree)

 