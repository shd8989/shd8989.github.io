---
title: docker command
author: Hyundong
date: 2023-10-19 17:50:00 +0800
categories: [docker, command]
tags: [docker]
---

## network

| 명령어 | 설명 |
|------|---------------|
| docker network create --driver bridge<br> 네트워크명 | 네트워크 종류 |
| | bridge - 하나의 호스트 컴퓨터 내에서 여러 컨테이너들이 서로<br> 통신하기 위해서 사용 |
| | host - 컨테이너를 호스트 컴퓨터와 동일한 네트워크에서<br> 컨테이너를 돌리기 위해서 사용 |
| | overlay - 여러 호스트에 분산되어 돌아가는 컨테이너들 간에<br> 네트워킹을 위해서 사용 |
| docker run -d --net=isolated_network --name<br> mongodb mongo |

## build

| 명령어 | 설명 |
|------|---------------|
| docker build -t IMAGE명:버전 -f 도커파일명<br> dockerfile경로 | Dockerfile이 있는 현재 위치에서 해당 파일을 ag_pgp:2.1.3-3.7<br>이라는 이미지로 빌드 |
| | 도커파일명을 Dockerfile로 하면 이름을 명시하지 않아도 됨 |
| docker build -t ag-pgp:2.1.3-3.7 -f Dockerfile . | |

## 컨테이너 생성 및 실행

| 명령어 | 설명 |
|------|---------------|
| docker run --name 컨테이너명 -it -d<br> -p 호스트PORT:컨테이너PORT 이미지명:버전명 bash | |
| docker run --name ag-pgp-master -it -d -p 4321:5432<br> -p 9698:9898 -p 9003:9000 -p 9594:9694 -p 11213:11211<br> ag-pgp:2.1.3-3.7 bash |  |
| docker run --privileged --name ag-pgp-master -it -d<br> -p 4321:5432 -p 9698:9898 -p 9003:9000 -p 9594:9694<br> -p 11213:11211 ag-pgp:2.1.3-3.7 bash | docker 실행시 기본적으로 unprivileged 모드로<br> 실행된다. |
| | unprivileged 모드로는 시스템 자원에 대한 권한이<br> 없어서 컨테이너 내 시스템 자원을 제어하고<br> 싶다면 privileged 옵션을 추가해야 한다. |
| | --memory=32g --memory-swap=64g |
| docker run --privileged -i -v /home/agens/data/docker_data<br>:/home/agens/data:z --name agensgraph -it -d -p 15432:5432<br> agensgraph:2.12.0 bash | |
| | -i, -v :z 또는 :Z를 통해 host의 디렉토리와 <br>container의 디렉토리를 공유할 수 있다. |
| | :z는 모든 컨테이너와 공유 가능 |
| | :Z는 특정 컨테이너만 공유 가능 |
| docker run -v /etc/localtime:/etc/localtime:ro<br> -e TZ=Asia/Seoul -name 컨테이너명 이미지명:버전명 bash | |
| | timezone 설정 |
| | 아래 명령어를 통해 host 서버에서 timezone <br>심볼릭 링크 생성 |
| | ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime |

## 상태 확인

| 명령어 | 설명 |
|------|---------------|
| docker stats | 실행중인 컨테이너 자원 할당 정보 확인 |

## 업데이트

| 명령어 | 설명 |
|------|---------------|
| docker update --memory 32g --memory-swap 32g | 컨테이너에 메모리 할당같은 추가 작업 진행 |

## 이미지 커밋

| 명령어 | 설명 |
|------|---------------|
| docker commit 컨테이너명 이미지명:태그명 | 태그명은 생략해도 된다 |

## 컨테이너 실행

| 명령어 | 설명 |
|------|---------------|
| docker exec -it --user root 8075310ae4cd bash | root 계정으로 실행 |
| docker exec -it $(docker run --privileged -p 10000:10000<br> -d hive init) bash | hive라는 이미지를 comment에 init으로 주어 <br>바로 container 실행 |

## 이미지 커밋

| 명령어 | 설명 |
|------|---------------|
| docker commit 컨테이너명 이미지명:태그명 | 태그명은 생략해도 된다 |

## 파일 복사

| 명령어 | 설명 |
|------|---------------|
| docker cp 호스트파일경로 컨테이너명:컨테이너<br>파일경로 | 호스트 → 컨테이너 |
| | 디렉토리도 마찬가지(-r, -R같은 옵션 필요 없음) |
| docker cp 컨테이너명:컨테이너파일경로 호스트<br>파일경로 | 컨테이너 → 호스트 |
| | 디렉토리도 마찬가지(-r, -R같은 옵션 필요 없음) |

## 이미지 <-> 파일

| 명령어 | 설명 |
|------|---------------|
| 1. docker save (docker image -> tar) | docker 이미지를 tar파일로 저장하기 위해서는 docker<br> save 커맨드를 사용한다 |
| docker save [옵션] [파일명] [이미지명] | |
| docker save -o nginx.tar nginx:latest | 저장할 파일명을 지정하는 옵션은 -o 를 사용한다. |
| 2. docker load (tar -> docker image) | tar파일로 만들어진 이미지를 다시 docker image로<br> 되돌리기 위해서는 docker load 커맨드를 사용한다. |
| docker load -i tar파일명 | |
| 3. docker export (docker container -> tar) | docker는 이미지 뿐 아니라 container를 tar파일로<br> 저장하는 명령어를 제공한다. |
| docker export [컨테이너명 또는 컨테이너ID] xxx.tar | |
| docker export af4282c7fba9 -o age-1.1.0.tar | |
| 4. docker import (tar -> docker image) | export 커맨드를 통해 만들어진 tar 파일을 다시 docker
image로 생성하는 명령어이다 |
| docker import [파일 또는 URL] [이미지명[:태그명]] | |
| docker import age-1.1.0.tar age:1.1.0 | |

## docker system full

| 명령어 | 설명 |
|------|---------------|
| docker system prune | |

## 에러 확인

| 명령어 | 설명 |
|------|---------------|
| dockerd --debug | |

## 에러복구

| 명령어 | 설명 |
|------|---------------|
| update-alternatives --set iptables /usr/sbin/iptables-legacy | iptables 버전 복구 |
| update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy | |

## 도커복구

| 명령어 | 설명 |
|------|---------------|
| dockerd | |