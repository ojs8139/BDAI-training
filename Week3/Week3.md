# Week3

## Cloudera Admin Official Video Lecture
* Day-1
* Day-2
* Day-3
* Day-4
3주차 Weekly Report 초안입니다.

---
## 2019/03/18 Mon
## 빅데이터란?
하둡으로 처리할 데이터
```
대용량 데이터 처리 : Tera-Byte(10^12)
```
## 하둡이란?
데이터를 분산 저장하고 분산된 데이터를 병렬 처리하는 시스템
```
HDFS : 분산 저장
YARN : 병렬 처리
```

## Cloudera Manager
클라우데라 메니저는 시스템을 관리할 수 있게 해주는 클러스터 메니지먼트 툴
하둡은 하나의 시스템을 쓰는게 아니라 여러 개의 서버를 뭉쳐 놓은 걸 클러스터라 하는데, 클러스터를 관리하고 운영해야 하는데 이를 하는게 클라우데라 메니저이다.
Ambari 는 Hue + 클라우데라 매니저

클라우데라 매니저 관리자 툴은 클라우데라 CM, 클러스터 관리
클라우데라 유저 인터페이스는 Hue

하이브는 맵리듀스로 돈다.

HA : 고가용성, 하나가 죽어도 또 쓸수 있는 형태로 만드는 것

## 데이터의 변화
Moore's Law : 컴퓨터 성능이 매 18개월 마다 두 배가 된다.

## Hadoop Cluster
하둡 대몬들은 각 각의 클러스터 안의 기계 안에서 돌아가고 있다. HDFS의 지정된 저장소는 없다. 개념이다. 데이터 자체를 불러오는게 아닌 데이터가 있는 곳을 불러오는 구조이다.

Hadoop의 개념
```
네임노드에 metadata가 정의되어 있으며, 각각의 호스트에 파일들을 분산하여 저장한다. 각각의 호스트의 저장 디렉토리에 파일은 열어볼 수 없다.(나눠져 있어서)
```
클러스터 관리 툴<br>
Cloudera - Cloudera Management
Hontonworks - Ambari

## Cloudera Manager
기능 : 자동으로 개발 가능, 로그 조회 간단, 하둡과 에코시스템 서비스 지원이 다양하다. 쉽게 서비스 STOP/START 할 수 있다.

Agent : 각각의 서버에서 돌고 있는 일종의 스파이. 각각의 노드들의 정보를 받아오고 CM 서버에 전송해주는 역할을 한다.

Secondary Name Node : CheckPoint 서버라고 생각하면 된다.

Active NameNode
Standby NameNode 조사할 것

설치시
Master 기능 : ResourceManager, JobHistoryServer, SecondaryNameNode(non-HA env), NameNode, NodeManager 등은 각각 서버를 따로 두어야 된다.

DataNode : 저장기능
NodeManager : 저장된 걸 병렬처리하는 기능
그래서 이 두개는 같은 서버에 올려도 된다.

Secondary NameNode 는 non-HA enviroment에서 구동된다. HA env 면 사라지는 노드.

## HDFS : The Hadoop Distributed File System
HDFS 자체가 하나의 어플리케이션이다. 파일을 HDFS에서 PUT을 하는 순간 나눠주는 역할을 하고, ls 하는 순간 NameNode로 가서 그 위치를 뿌려주는 역할을 한다. 자바로 이루어져 있다.
로딩하는 순간에 데이터가 분산됨.

2000 노드 이상이면 무조건 1 노드는 죽는다는 가정하에 설계됨.
* metadata 이름 : fsimage, edits<br>
