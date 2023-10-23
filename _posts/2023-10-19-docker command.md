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
| docker network create --driver bridge 네트워크명 | 네트워크 종류 |
| | bridge - 하나의 호스트 컴퓨터 내에서 여러 컨테이너들이 서로 통신하기 위해서 사용 |
| | host - 컨테이너를 호스트 컴퓨터와 동일한 네트워크에서 컨테이너를 돌리기 위해서 사용 |
| | overlay - 여러 호스트에 분산되어 돌아가는 컨테이너들 간에 네트워킹을 위해서 사용 |
| docker run -d --net=isolated_network --name mongodb mongo |

## build

| 명령어 | 설명 |
|------|---------------|
| docker build -t IMAGE명:버전 -f 도커파일명 dockerfile경로 | Dockerfile이 있는 현재 위치에서 해당 파일을 ag_pgp:2.1.3-3.7이라는 이미지로 빌드 |
| | 도커파일명을 Dockerfile로 하면 이름을 명시하지 않아도 됨 |
| docker build -t ag-pgp:2.1.3-3.7 -f Dockerfile . | |

## 컨테이너 생성 및 실행

| 명령어 | 설명 |
|------|---------------|
| docker run --name 컨테이너명 -it -d -p 호스트PORT:컨테이너PORT 이미지명:버전명 bash | |
| docker run --name ag-pgp-master -it -d -p 4321:5432 -p 9698:9898 -p 9003:9000 -p 9594:9694 -p 11213:11211 ag-pgp:2.1.3-3.7 bash |  |
| docker run --privileged --name ag-pgp-master -it -d -p 4321:5432 -p 9698:9898 -p 9003:9000 -p 9594:9694 -p 11213:11211 ag-pgp:2.1.3-3.7 bash | docker 실행시 기본적으로 unprivileged 모드로 실행된다. |
| | unprivileged 모드로는 시스템 자원에 대한 권한이 없어서 컨테이너 내 시스템 자원을 제어하고 싶다면 privileged 옵션을 추가해야 한다. |
| | --memory=32g --memory-swap=64g |
| docker run --privileged -i -v /home/agens/data/docker_data:/home/agens/data:z --name agensgraph -it -d -p 15432:5432 agensgraph:2.12.0 bash | |
| | -i, -v :z 또는 :Z를 통해 host의 디렉토리와 container의 디렉토리를 공유할 수 있다. |
| | :z는 모든 컨테이너와 공유 가능 |
| | :Z는 특정 컨테이너만 공유 가능 |
| docker run -v /etc/localtime:/etc/localtime:ro -e TZ=Asia/Seoul -name 컨테이너명 이미지명:버전명 bash | |
| | timezone 설정 |
| | 아래 명령어를 통해 host 서버에서 timezone 심볼릭 링크 생성 |
| | ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime |