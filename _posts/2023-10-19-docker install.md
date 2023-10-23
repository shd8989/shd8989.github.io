---
title: docker install
author: Hyundong
date: 2023-10-19 17:21:00 +0800
categories: [docker, install]
tags: [docker]
---

# 도커 설치
## CentOS 7
필수 패키지 설치
```console
yum -y install yum-utils
```
```console
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

도커 설치
```console
yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

도커 실행
```console
systemctl start docker
```

유저 권한 부여
```console
usermod -aG docker agens
```

## Ubuntu 20.04
필수 패키지 설치
```console
apt-get update && apt-get install ca-certificates curl gnupg lsb-release
```

gpg key 추가
```console
mkdir -m 0755 -p /etc/apt/keyrings && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

repository 설정
```console
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

repository update
```console
apt-get update
```

도커 설치
```console
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

유저 권한 부여
```console
usermod -aG docker agens
```