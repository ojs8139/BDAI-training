# Week2

## Technical Seminar
* Bigdata
* Hadoop
* MapReduce
* YARN

---
## 빅데이터란?
```
폭발적으로 커져버린 데이터에 대한 새로운 데이터 처리방식이 필요하게 되었고 이러한 흐름에 따라 분산처리 플랫폼(Hadoop)등으로 처리해야 하는 데이터
```
### 데이터의 변화 속도
Moore's Law : 컴퓨터 성능이 매 18개월 마다 두 배가 된다.
### 빅데이터의 특성
Volume : 데이터의 물리적인 크기<br>
Velocity : 데이터 처리 속도, 능력<br>
Variety : 정형, 비정형의 다양한 데이터

## Hadoop
대용량 데이터를 분산 처리할 수 있는 자바 기반의 오픈소스 프레임워크<br>
분산 처리 : 다수의 기기들과 드라이브에 분산하여 데이터를 저장

네임노드에 metadata가 정의되어 있으며, 각각의 호스트에 파일들을 분산하여 저장한다. 각각의 호스트의 저장 디렉토리에 파일은 열어볼 수 없다.(나눠져 있어서)

----
### Hadoop 구성요소
```
Core : HDFS, YARN / MapReduce
Ecosystem : 코어 이 외의 모든 시스템  
```

## HDFS
### 특징
* 실시간 데이터 처리 결과 조회 보단 한 번에 많은 작업 처리하는 배치 프로세스에 맞게 설계
* WORM(Write Only Read Many)
* 1Block = 128M(하둡 2.0 이상)
* 용량이 클 수록 저장하기 유리

### 구성요소
```
HDFS에는 마스터 역할을 하는 네임노드 서버 한 대와, 슬레이브 역할을 하는 데이터노드 서버가 여러 대로 구성된다.
```
NameNode

HDFS의 모든 메타데이터(블록들이 저장되는 디렉토리의 이름, 파일명등..)를 관리하고, 클라이언트가 이를 이용하여 HDFS에 저장된 파일에 접근할 수 있다.

DataNode

주기적으로 네임노드에서 블록 리포트(노드에 저장되어 있는 블록의 정보)를 전송하고 이를 통해 네임노드는 데이터 노드가 정상 동작하는지 확인한다.

---
## MapReduce
대용량의 데이터 처리를 위한 분산 프로그래밍 모델, 소프트웨어 프레임워크

맵(Map)
```
흩어져 있는 데이터를 연관성 있는 데이터들로 분류하는 작업(key, value의 형태)
```

리듀스(Reduce)
```
Map에서 출력된 데이터를 중복 데이터를 제거하고 원하는 데이터를 추출하는 작업.
```
1. Splitting : 문자열 데이터를 라인별로 나눈다.

2. Mapping : 라인별로 문자열을 입력받아, <key, value> 형태로 출력.

3. Shuffling : 같은 key를 가지는 데이터끼리 분류.

4. Reducing : 각 key 별로 빈도수를 합산해서 출력.

5. Final Result : 리듀스 메소드의 출력 데이터를 합쳐서 하둡 파일 시스템에 저장.

### 맵 리듀스 시스템 구성
JobTracker는 NameNode에, TaskTracker는 DataNode에 위치한다.
* Client
* JobTracker
* TaskTracker

Client : 분석하고자 하는 데이터를 잡의 형태로 JobTracker에게 전달

JobTracker : 하둡 클러스터에 등록된 전체 job을 스케줄링하고 모니터링

TaskTracker : DataNode에서 실행되는 데몬이고, 사용자가 설정한 맵리듀스 프로그램을 실행하며, 
JobTracker로부터 작업을 요청받고 요청받은 맵과 리듀스 개수만큼 맵 태스크와 리듀스 태스크를 생성

---

## YARN(Yet Another Resource Nagotiator)
각 어플리케이션(HBase, Accumulo, Storm, MapReduce...)에 필요한 리소스(CPU, 메모리, 디스크 등)를 할당하고 모니터링하는 업무
```
MapReduce 는 JobTracker 와 TaskTracker를 한 곳에서 처리하지만, 이를 개선하여 Resource Management(자원관리) 와 job scheduling/monitoring(일정조율 및 모니터링)의 기능을 나눈 MapReduce 2 버전
```

JobTracker의 기능은 ResourceManager, Application Master의 두가지 프로세스로, 
TaskTracker의 기능은 NodeManager가 대신하게 하였다.

### YARN 구성요소
ResourceManager<br>
클러스터에 1개 존재하며, 클러스터의 전반적인 자원 관리와 스케쥴러와 Application Manager를 조정하는 역할을 한다.
클라이언트로부터 어플리케이션 실행 요청을 받으면, 그 실행을 책임질 Application Master를 실행한다.
클러스터 내의 Node Manager들과 통신을 해서 할당된 자원과 사용중인 자원의 상황을 알 수 있다.

Scheduler<br>
Node Manager들의 자원 상태를 관리하며 부족한 리소스를 할당한다.
자원 상태에 따라서 태스크들의 실행 여부를 허가해주는 역할인 스케쥴링 작업만 담당한다.
Node Manager들의 자원 상태를 Resource Manager에게 통지한다.

Application Manager<br>
Node Manager에서 특정 작업을 위해 Application Manager를 실행하고, Application의 실행 상태를 관리하여 그 상태를 Resource Manager에게 통지한다.

Node Manager<br>
노드 당 한개씩 존재하며, Container의 리소스 사용량을 모니터링하고, 관련 정보를 Resource Manager에게 알리는 역할을 한다.

Application Master<br>
YARN에서 실행되는 하나의 태스크를 관리하는 마스터 서버를 말한다. 어플리케이션 당 1개가 있다.
Scheduler로부터 적절한 Container를 할당 받고, 프로그램의 실행 상태를 모니터링, 관리한다.
 
Container<br>
CPU, Disk, Memory 등의 리소스로 정의된다.
모든 작업은 결국 여러개의 태스크로 세분화되고, 각 테스크는 하나의 Container에서 실행된다. 
필요한 자원의 요청은 Application Master가 담당하며, 승인 여부는 Resource Manager가 담당합니다. Container안에서 실행할 수 있는 프로그램은 자바프로그램뿐만 아니라, 커맨드 라인에서 실행할 수 있는 프로그램이라면 모두 가능하다.

### YARN Configuration
맵리듀스 설정 >> 우버 테스크 설정 >> 얀 스케줄러 설정 >> 노드메니저 추가 및 제거 >> 리소스매니저 H/A 구성

---

## Digital Twin
### 디지털 트윈의 정의
```
물리적 객체(자산, 프로세스 및 시스템 등)들에 대한 디지털 복제(쌍둥이)로서, 수명주기 전체에 걸쳐 대상 객체 요소들의 속성/상태를 유지하며 이들이 어떻게 작동하는지의 동적 성질을 묘사하는 가상의 모델
```

### Predix 플랫폼
미국 GE사의 Predix는 산업 인터넷 플랫폼을 통해 제조업 분야의 디지털 응용서비스 제공을 목적으로 개발된 디지털 트윈 개발/운영 플랫폼

### MindSphere
독일 Simens사의 MindSphere는 클라우드 기반 개방형 IoT 솔루션으로 전사적 정보의 취합, 처리·저장 및 분석을 지원하는 플랫폼

### 디지털 트윈의 적용
* 제조 분야<br>
제조 부문에서는 제품 설계에서부터 플랜트 운영 감시, 작업량/로드 예측, 생산 손실 예측, 고장 진단/예측 등 제조 공정의 효율성 제고 및 최적화

* 항공 및 전력 분야<br>
항공 및 전력 산업에서는 프로펠러, 터빈 등 기계의 고장 진단, 예측

* 자동차 분야<br>
자동차 분야에서 가상 모델은 차량의 성능을 분석하고 고객에게 맞춤식 경험을 제공하여 설계, 판매 등

* 의료 분야<br>
의료 분야에서 IoT 플랫폼의 데이터를 활용한 개인화된 의료 서비스 제공 및 효율적인 환자 모니터링 
* 스마트 시티<br>
스마트 도시에서는 해당 도시에 대한 교통, 에너지, 환경 등의 새로운 정책을 가상 도시(모델)를 통해 사전 검증

## 디지털 트윈 기술/응용 시사점
기본적으로 모델링 시뮬레이션 기술의 특성을 계승하는 디지털 트윈을 가능하게 하는 3가지 핵심 역량
- 사업 분야에 대한 전문적인 데이터 역량 - 데이터 수집 및 전처리 자동화, 디지털 쓰레드(digital thread) 등
- 다학제 공학적 물리 모델과 (3D)디지털 모델 도출 역량 - 제품의 특성, 수명, 거동 등을 묘사하는 모델
- 빅데이터 분석 및 지식 추출 역량 - 산업분야 응용목적에 맞는 분석, 예측 수행 어플리케이션 SW