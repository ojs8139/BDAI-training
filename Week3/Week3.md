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
네임노드에 metadata가 정의되어 있으며, 각각의 호스트에 파일들을 분산하여 저장한다. <br>각각의 호스트의 저장 디렉토리에 파일은 열어볼 수 없다.(나눠져 있어서)
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

---
# Day-2
## HDFS : The Hadoop Distributed File System
HDFS 자체가 하나의 어플리케이션이다. 파일을 HDFS에서 PUT을 하는 순간 나눠주는 역할을 하고, ls 하는 순간 NameNode로 가서 그 위치를 뿌려주는 역할을 한다. 자바로 이루어져 있다.
로딩하는 순간에 데이터가 분산됨.

### Hadoop 구조
Master/Slave 구조로 이루어져 있다.
NameNode - 메모리에서 관리함.

2000 노드 이상이면 무조건 1 노드는 죽는다는 가정하에 설계됨.
* metadata 이름 : fsimage, edits<br>

NameNode가 죽으면 서비스 불가
SecondaryNameNode가 죽으면 일시적으로 서비스는 가능
하지만 checkpoint가 점점 쌓여 오버플로우가 생김

Hadoop is "Rack-aware"
데이터를 저장할 때, 저장된 현재 rack 이 외의 다른 rack에 할당한다.

### NN CheckPoint
edits : 변동된 트랜젝션
fsimage : 현재 상태 스냅샷
현재 : edits, fsimage 존재
1. edits.new 하여 변동사항 edits로 로그 생성 준비
2. 현재 상태의 edits, fsimage를 SecondaryNode로 Copy
3. SecondaryNode에 복사된 edits와 fsimage를 합친다.
>> Newfsimage CheckPoint
4. 이렇게 만들어진 Newfsimage를 NameNode의 edits로
보낸다. <데이터 최신화>
5. 1~4번 과정 반복

### NN Startup
SafeMode(Read-Only Mode)
```
블럭이 깨지거나, 데이터가 가득찼을 때 뜨는데, 업데이트가 이루어지지 않는 모드를 말한다. 여기서 업데이트란 PUT, DELETE가 되지 않는 상태를 SafeMode라 한다.
```
관리자가 일부러 안전모드를 실행할 때가 있는데, NameNode가 Reboot 할 때이다.

NameNode 는 따로 서버를 두고 관리해야 한다.

## Hadoop Security
* Authentication<br>
사용자의 신분을 밝히는 것, 문을 들어올 수 있는 권한

* Authorization(access control)<br>
내용을 볼 수 있는 권한, 접근 권한, 쓸 수 있는 권한 등
Role-Base로 권한을 줄 수 있다.

* Data encryption<br>
OS-level, HDFS-level, Network-level 데이터 보안이 유지될 수 있도록 한다.

## HDFS Web UI
HDFS의 전체적인 내용을 조회되는 화면
* 클라우데라 매니저
* 네임노드 화면(port 50070)
* HUE
* CLI 터미널 화면
* Java API
* REST API

## HDFS Shell
HDFS 디렉토리 조회
```
$ hdfs dfs -ls
```

## Accessing HDFS Command Line
### Display contents
```
$ hdfs dfs -cat /user/fredsales.text file
```

### Create a directory called reports
```
$ hdfs dfs -mkdir /reports
```

### Delete the file
```
$ hdfs dfs -rm /reports/sales.txt
```

### PUT
command copies local files to HDFS
```
$ hdfs dfs -put sales.txt /report
```

### GET
fetches a local copy of a file from HDFS
```
$ hdfs dfs -get /reports/sales.txt
```

### Linux 기본 명령어
- -rm
- put, get
- mkdir
- cat
- ls
- mv
- cp
### 꼭 숙지할 것!!

## Apache HBase
대량의 데이터를 테이블 형태로 관리할 때 용이, 검색 빠르다.

* No-SQL database이다.
* 컬럼형 데이터베이스를 저장할 때 사용한다.
* HDFS와 연동한다.
* 핸들링할 수 있는 데이터 사이즈가 Kudu에 비해 작다.

Random Write, Random Read 기능 지원

## Apache Kudu
구조화된 데이터를 저장하는 엔진

* hdfs에 종속되지 않는다.
* Kudu + Impala를 쓸 때 효과가 극대화된다.
* HDFS와 HBase의 중간 정도의 효율을 낸다.
* 데이터 접근 방식이 로컬 파일 시스템이다. HDFS 아님.
* Random Access scan 퍼포먼스가 좋다.
* 데이터 노드에 설치하던가, 다른 곳에 설치
* SSD에 설치해도 된다.


# MapReduce

* 레코드 베이스의 key, value 형태로 이루어진 프로그래밍 모델
* 분산병렬로 처리할 수 있도록 여러 노드들에 대해 일을 나눌 수 있는 기능
* 데이터가 있는 노드에 위치
* Mapper - Shuffle & sort - Reduce 과정
## Mapper
레코드를 읽어서 key, value 쌍으로 만들어주는 역할

## Shuffle & sort
Mapper에서 나온 결과를 해당되는 Reduce로 자동으로 던져 주는 역할

## Reduce
같은 키에 있는 값들에 대해 operation을 일으키는 역할

## MapReduce Process
Input Data가 오면 Input Format에 따라 Input Split(각각의 블럭)으로 나눠진다. 이 블럭들이 각각 RecordReader에서 읽어지면 각각의 Mapper로 할당 되고, shuffle & sort가 받아서 해당되는 Reduce로 받아 그 결과를 Format에 맞춰 출력을 하여 결과들을 합치면 원하는 결과가 나온다.

## MapReduce 장점
* 병렬과 분산을 자동으로 처리
* Fault-tolerance
* A clean abstraction for programmers

## MapReduce Concepts
* Reduce의 갯수는 key 수에 따라 정하는게 일반적이다.
프로그래밍, 시스템, 명령어를 줘서 갯수를 정의할 수 있다.
* 각각의 Record에는 key, value가 있다.
* 중간 결과 과정을 local disk에 저장한다. disk가 가득차면 멈춰버린다. 중간결과는 나중에 필요없으니 로컬에 저장
* Reduce 결과는 hdfs에 쓴다.

## MapReduce Application Terminology
* Job 
```    
하나의 어플리케이션. 여러개의 Task로 구성.<br>Task는 MapTask, ReduceTask로 이루어짐.
맵리듀스의 high-level 객체
```
* Task 
```
하나의 Job이 여러개의 Task로 구성
Mapper 하나가 하나의 Task이고 MapTask, ReduceTask 중 하나로 구성
Task가 구동되려면 Container가 있어야함.
```
### Container
```
CPU와 Memory의 조합
```
---
# YARN Daemons
클러스터에 하나씩 배정
```
ResourceManager
JobHistoryManager
```
NodeManager 는 worker node에 하나씩 배정

---
# Sqoop
RDBMS와 Hadoop database(Hive, Avro, Parquet, HBase, Accumulo) 데이터를 주고 받을 수 있는 툴

## Sqoop 동작 방식
Sqoop 동작시 map 으로 이루어진 자바 프로그램이 돈다.

# Day 3
## 하이브 아키텍쳐와 데이터 모델
### 하이브 개요
    - SQL과 유사한 HiveQL을 사용
    - 맵리듀스 프로그램 작성 대신 쿼리 인터페이스 서비스 제공
    - 쿼리 실행 시에 맵리듀스 프로그램으로 전환되어 결과를 생성함
    - RDBMS처럼 테이블을 이용하여 쿼리를 수행하므로 비정형화된 입력 소스 분석에는 적합하지 않음
### 하이브 클라이언트
    JDBC 기반 응용 프로그램 지원
    Thrift 기반 응용 프로그램 지원
    ODBC 기반 응용 프로그램 지원

### 하이브 서비스
    하이브 서버
    CLI
    Hive Web Interface
    Driver
    Metastore
    Apache Derby Database

### 하이브 쿼리 명령어 인터페이스
    하이브 클라이언트(Hive CLI)
    Beeline