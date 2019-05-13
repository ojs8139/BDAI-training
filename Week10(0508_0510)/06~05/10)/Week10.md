# 신입사원 TEST 결과물
## 설치 PNG 파일은 된 화면까지 캡쳐하여 첨부하였습니다.
![](2019-05-13-15-28-30.png)

2.a create database test;
use test;

2.b 
create table authors (변수명 타입 정의);
create table posts (변수명 타입 정의);

2.c_1. create user 'Cert'@'%' identified by 'Cert';
2.c_2.GRANT ALL PRIVILEGES ON test.* To 'Cert'@'%' IDENTIFIED BY 'Cert';

### 3번 문제해결 순서
hive에 table 생성 후 sqoop 임포트(hive import arg 사용)

CREATE TABLE authors(변수명 타입 정의) FIELDS TERMINATED BY ',' stored as parquet;
CREATE TABLE posts(변수명 타입 정의) FIELDS TERMINATED BY ',' stored as parquet;

sqoop import 
--conect jdbc:mysql://192.168.56.210:3306/test
--username Cert --password "Cert"
--table authors
--hive-import --hive-database default --hive-table authors

sqoop import 
--conect jdbc:mysql://192.168.56.210:3306/test
--username Cert --password "Cert"
--table authors
--hive-import --hive-database default --hive-table posts

3.c 
hdfs dfs -mkdir /authors
hdfs dfs -mkdir /posts

### 4번 문제해결
select count(*) from authors
select count(*) from posts

select authors.id as id, authors.first_name as fname, authors.last_name as Lname, count(*) 
from authors 
join posts 
on (authors.author_id = posts.id)

#### on mysql
mysql에서 create table results(변수명 타입);
