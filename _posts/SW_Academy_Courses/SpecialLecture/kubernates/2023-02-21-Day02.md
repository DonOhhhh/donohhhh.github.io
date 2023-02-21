---
categories: [SWACADEMY]
---

# Day02

## Docker란 무엇인가?

Docker는 애플리케이션을 신속하게 구축, 테스트 및 배포할 수 있는 소프트웨어 플랫폼이다. Docker는 소프트웨어를 컨테이너라는 
표준화된 유닛으로 패키징하며, 이 컨테이너에는 라이브러리, 시스템 도구, 코드, 런타임 등 소프트웨어를 실행시키는 데 필요한 모든 것이 
포함되어 있다.

### Docker 용어

#### Docker 이미지

Docker 이미지에는 실행 가능한 애플리케이션 소스 코드는 물론, 애플리케이션 코드가 컨테이너로서 실행해야 하는 모든 툴, 라이브러리 및 종속 항목들이 포함되어 있다. 
Docker 이미지를 실행하는 경우, 이는 컨테이너의 하나의 인스턴스(또는 다수의 인스턴스)가 된다.

#### Docker hub

Docker 허브는 스스로 "컨테이너 이미지의 세계 최대 라이브러리 및 커뮤니티"라고 부르는 Docker 이미지의 공용 저장소이다.
이는 상용 소프트웨어 공급업체, 오픈 소스 프로젝트 및 개별 개발자들로부터 제공받은 100,000개 이상의 컨테이너 이미지를 보유하고 있다. 
여기에는 Docker, Inc에서 생성한 이미지, Docker Trusted Registry에 속하는 공인 이미지, 그리고 수천 개의 기타 이미지들이 포함된다.

### Docker Daemon

Docker 데몬은 Windows 또는 MacOS 등의 운영체제에서 실행되는 서비스이다. 이러한 서비스는 Docker 구현의 제어 센터 역할을 하는 클라이언트의 명령을 사용하여 사용자 대신 Docker 이미지를 작성하고 관리한다.

#### Docker Registry

Docker Registry는 Docker 이미지의 확장 가능한 오픈 소스 스토리지 및 분배 시스템이다. 레지스트리를 사용하면 식별을 위한 태깅을 사용하여 저장소에 이미지 버전을 추적할 수 있다. 이는 버전 제어 툴인 git을 사용하여 수행된다.

### Docker Architecture

![Docker Architecture](/assets/images/2023/02/21/img.png)

#### Kubernetes

보다 복잡한 환경에서 컨테이너 라이프사이클을 모니터하고 관리하려면 컨테이너 오케스트레이션 툴에 의존해야 한다. Docker에 자체 오케스트레이션 툴(Docker Swarm)이 포함되어 있지만, 대부분의 개발자는 그 대신 Kubernetes를 선택한다.
Kubernetes는 Google에서 내부용으로 개발된 프로젝트에서 파생된 오픈 소스 컨테이너 오케스트레이션 플랫폼이다.
Kubernetes는 컨테이너 배치, 업데이트, 서비스 감지, 스토리지 프로비저닝, 로드 밸런싱, 상태 모니터링 등을 포함하여 컨테이너 기반 아키텍처의 관리에 필수적인 태스크를 스케줄링하고 자동화한다.

## Semantic Versioning

버전을 주.부.수 숫자로 정한다.

주 : 기존 버전과 호환되지 않게 API가 바뀌면 "주(主) 버전"을 올린다.
부 : 기존 버전과 호환되면서 새로운 기능을 추가할 때는 "부(部) 버전"을 올린다.
수 : 기존 버전과 호환되면서 버그를 수정한 것이라면 "수(修) 버전"을 올린다.

## 실습

### 1. Docker에서 MySQL 실행하기

```
# mysql 이미지 다운
docker pull mysql

# 컨테이너 생성 및 실행
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<password> -d -p 3306:3306 mysql:latest

# 실행 중인 컨테이너에 쉘로 접속
docker exec -it mysql-container /bin/bash

# 컨테이너 시작
docker start mysql-container

# 컨테이너 종료
docker stop mysql-container

# 컨테이너 재시작
docker restart mysql-container

# 실행 중인 컨테이너 목록
docker ps

# docker 이미지 목록
docker images
```

```
# Docker MySQL image로 컨테이너 접속
mysql -u root -p

# 데이터베이스 확인
show databases;

# docker_users라는 데이터베이스 생성 및 확인
create database docker_users;
show databases;

# 해당 데이터베이스에 접속
use docker_users;

# 테이블 생성
create table users (
  id int not null auto_increment primary key,
  name varchar(55) not null
);

# 데이터 삽입
insert into users (name) values ('chungnam');
insert into users (name) values ('university');

# 데이터 조회하기
select * from users;
```

### 2. Dockerfile을 이용하여 dockerize 해보기

1. helloworld 파일 생성(만약 windows 사용자라면 vscode에서 아래 그림과 같이 설정하여 진행해야 오류가 안 생김.)

![windows에서 제작한 shell script linux에서 실행시킬 때 오류 해결 방법](/assets/images/2023/02/21/img_1.png)

```
#! /bin/sh
echo "Hello, World!"
```

2. Dockerfile 작성

```
FROM alpine:latest
COPY helloworld /usr/local/bin
RUN chmod +x /usr/local/bin/helloworld
CMD ["helloworld"]
```

3. Docker image 빌드 및 실행

```
docker image build -t helloworld:latest .
docker run helloworld
```

![실행 결과](/assets/images/2023/02/21/img_2.png)

### 3. Dockerfile을 이용하여 생성한 image를 docker hub에 업로드하기

1. docker hub에 가입하기
2. dockerfile 만들기

```
FROM ubuntu:latest
LABEL MAINTAINER=ohdoju0905@gmail.com
CMD ["echo", "Hello, CMU World!"]
```

3. docker image 만들기

```
docker build -t donoh/cmu:0.0.2 .
```

4. docker hub에 이미지 올리기

```
docker login
# push를 하려면 donoh의 cmu라는 docker repository가 이미 만들어져 있어야 함.
docker push donoh/cmu:0.0.2
```

5. container 생성 및 실행

```
docker run donoh/cmu:0.0.2
```

### 4. Docker로 웹 어플리케이션 실행하기

1. app.py 제작

```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
  return "Hello World!\n"
  
if __name__== "__main__":
  app.run(host="0.0.0.0")
```

2. Dockerfile 생성

```
FROM python:3.8-slim
COPY . /app
RUN pip3 install flask
WORKDIR /app
CMD ["python3", "-m", "flask", "run", "--host=0.0.0.0"]
```

3. Docker image 빌드 및 실행

```
docker build -t flask_test .
docker run -p 5001:500 flask_test
```

4. 해당 포트로 웹 서버 접근

```
curl localhost:5001
```

![실행 결과](/assets/images/2023/02/21/img_3.png)