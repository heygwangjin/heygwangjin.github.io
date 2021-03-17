---
layout: post
title: "[Database] 맥 OS 에서 터미널을 이용하여 postgreSQL 사용하기"
tags: Database 
categories: Database 
---
학교 데이터베이스 수업에서 postgreSQL DBMS를 사용하는데 pgadmin이라는 GUI(Graphical User Interface)를 통해서 postgreSQL을 사용한다. 그러나 맥에서 GUI가 아닌 CLI 환경을 통해서 postgreSQL을 사용해 보고 싶어서 초기 설정에 대해서 포스팅으로 남기고자 한다.  

본 포스팅에서는 맥 OS 패키지 관리자인 [Homebrew](https://brew.sh/index_ko)를 통해 postgreSQL을 설치하고 사용 방법을 알아본다.
## postgreSQL 설치
* * *
terminal.app 을 실행하여 아래와 같은 명령어를 사용한다.  

`brew install postgresql`  

설치가 완료 되었으면 아래 명령어를 이용해 postgreSQL 서비스를 시작한다.  

`pg_ctl -D /usr/local/var/postgres start && brew services start postgresql`

서비스가 정상적으로 실행됐는지 확인하기 위해, 아래 명령어를 사용하여 확인한다.  
`postgres -V`

## postgreSQL CLI 사용하기 
* * *
postgreSQL 을 설치했다. postgreSQL 에서는 psql이라는 명렁어를 통해서 postgreSQL의 CLI를 사용할 수 있다. psql 명령어를 사용할 때, 데이터베이스 이름도 같이 작성해야한다. 기본적으로 postgres 라는 데이터베이스가 생성되어 있다.  
### postgres 데이터베이스에 접속하기
우선 psql 명렁어로 postgreSQL의 postgres라는 데이터베이스에 접속한다.  

`psql postgres # postgreSQL을 설치하면 자동으로 생성되는 데이터베이스이다.`  

![postgres](/images/postgres.png)
postgreSQL의 postgres 데이터베이스 쉘(?)에 접속했다.

## postgreSQL 간단한 명령어 알아보기 
* * *
PostgreSQL을 CLI 환경에서 다루기 위한 간단한 명령어를 알아보자. 

### 데이터베이스 목록 확인하기
현재 데이터베이스의 목록을 확인하는 명령어는 \l (역슬래시 소문자 L)이다. 
![list](/images/postgres-list.png)

위와 같이 3개의 데이터베이스가 존재한다. postgres, template0, template1 이다. 우리는 지금 postgres 라는 데이터베이스에 접속해 있다. 내가 지금 접속해 있는 데이터베이스를 확인하려면 **postgres=#** 이 부분을 보면 된다.

### 사용하는 데이터베이스 변경하기
사용하는 데이터베이스를 다른 데이터베이스로 변경해 보자. \c [바꿀 데이터베이스명] 명령어를 입력한다.
![change](/images/postgres-change.png)
정상적으로 데이터베이스가 변경되었으며, 쉘의 template1=# 에서 이를 확인할 수 있다.
### 데이터베이스 사용자 정보와 권한 확인하기
 
\du 명령어를 통해서 ROLE과 권한을 확인할 수 있다.
![user](/images/postgres-userinfo.png)

마지막으로 \q 명렁어를 통해 postgreSQL 쉘 접속을 종료할 수 있다.

<h2 id="ref">Reference</h2>
* * *
[맥 OS 에서 PostgreSQL 설치하기](https://semtax.tistory.com/12)
