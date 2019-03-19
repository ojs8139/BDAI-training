# Week1

* 신입사원 교육
  + 신입사원 OT(Orientation) 동영상 시청
  + 네트워크 기본 개념,리눅스 가상환경 구성
  + 업무용 Github 환경 구성

* 기술 발표 참관
  * Flink
  * Ansible
  * Hue Load Balancer

## SE Linux
리눅스에서 사용되는 커널 레벨의 보안 모듈
vi /etc/sysconfig/selinux

## 방화벽 설정
systemctl stop firewalld
systemctl disable firewalld

방화벽이란 네트워크 트래픽을 모니터링하고 인바운드/아웃바운드에서
정의된 규칙을 기반으로 특정 포트를 허용/차단하는 보안 디바이스

## NAT(Network Address Translation) 란?
리눅스에서는 iptables를 보통 사용함.
외부에서 내부 호스트?로 네트워크 주소 할당 시 주소를 바꾸어줌
보안, ip 부족 때문
iptables -l
system-config-firewalld

## Flink 이란?
- 스트리밍 & 배치 프로세싱 데이터 분산 처리 플렛폼
- 실시간 처리, 수정 가능

## Spark
- Micro-batch
- 데이터가 쌓이고 처리
- 반복 안됨

## Flink
- 실시간
- 즉시 처리 가능
- 반복 허용

## Ansible 이란?
- 메인 컴퓨터를 두고 여러 컴퓨터의 환경 구성을 처리할 수
있는 자동화 도구(오픈소스)
- YAML : 데이터 직렬화 양식, 구조화된 데이터를 특정
포멧으로 변환하는 일종의 언어

### Ansible 구성 요소
- Inventory : 제어될 서버 목록 정의, 특징은 호스트
그룹핑 가능
- PlayBook : 서버 기능 정의, 태그 이용으로 제한/허용
- Module : 서버 기능을 어떻게 구현할 지 정의

### Ansible 장점
- agent 불필요
- 멱등성 : 한 번 실행된 동일한 소스는 실행 안함

## Hue Load Balancer 란?
HA(High Availability) : 서버와 네트워크, 프로그램 등이
지속적으로 운영 가능한 특징

Hue 프로세스 : Hue 서버에서 모든 Load Balancer에 신호를
보내면 그 중 가장 반응이 빠른 Load Balancer를 띄움

Fail Over : 서버 오류 시 다른 서버로 재할당 시키는 과정 및 결과
AD(Active Directory) : 보안, 권한 등을 관리하는 디렉토리

rdp(Remote Desktop Protocol)
- Xwindow 환경에서 원격 접속 프로토콜
- 다른 컴퓨터에 GUI를 제공함

sftp(Secure File transfer protocol)
- 전송시 암호화 시켜서 파일 전송
- 사용 포트는 기본 FTP 포트(21)이 아닌 SSH 접속 시 사용되는
포트 사용

ftp(file transfer protocol)
 - 파일 전송 프로토콜

telnet
 - 터미널 접속 프로그램(유닉스, 리눅스)
 - 원격지의 서버(컴퓨터)를 마치 직접 콘솔(console)앞에서 사용하는 것처럼 작업할 수 있도록 해주는 도구

패킷
 - 데이터를 일정 크기로 자른 것
 - 데이터를 프레임은 헤더, 페이로드, 트레일러로 구분 하여 자른 것?
 