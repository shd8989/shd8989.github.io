---
title: VirtualBox
author: Hyundong
date: 2023-11-11 01:31:00 +0800
categories: [virtualbox, vitual machine, install]
tags: [virtualbox]
---

# 설치전
## 저장소 설정
설치하려는 OS의 ISO파일 추가

## 네트워크 설정
어댑터에 브리지로 설정

## 공유 폴더 설정
폴더 경로는 host의 공유 폴더
마운트 지점은 guest의 공유 디렉토리

## Repository 설정(Minimal 설치일 경우)
vi /etc/yum.repos.d/CentOS.repo
```
## CentOS 6버전
[base]
name=CentOS Base
baseurl=http://centos.mirror.cdnetworks.com/6/os/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

[updates]
name=CentOS Updates
baseurl=http://centos.mirror.cdnetworks.com/6/updates/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

[extras]
name=CentOS Extras
baseurl=http://centos.mirror.cdnetworks.com/6/extras/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
```

```
## CentOS 7버전
[base]
name=CentOS Base
baseurl=http://mirror.navercorp.com/centos/7/os/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS Updates
baseurl=http://mirror.navercorp.com/centos/7/updates/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[extras]
name=CentOS Extras
baseurl=http://mirror.navercorp.com/centos/7/extras/x86_64
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

- CentOS 7 버전으로는 같은 mirrorlist에서 다운시 gcc 에러 발행하여 아래 명령어 추가

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
sed -i  's/$releasever/7/g' /etc/yum.repos.d/CentOS-Base.repo
yum -y install gcc
```

에러) device eth0 does not seem to be present delaying initialization
{: .font-red }
VIrtual Box 복제시 MAC주소 이상에 따른 오류
아래 두개 파일에서 MAC주소 변경
1. /etc/udev/rules.d/70-persistent-net.rules
2. /etc/sysconfig/network-scripts/ifcfg-eth0

## 파일 전송
### wget을 통한 다운로드
```
# yum -y install wget
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

## ISO Package mount
1. mount /dev/cdrom1 /cdrom
2. mkdir -p /root/local-repo/CentOS-6
3. cp -rf /cdrom/Packages /root/local-repo/CentOS-6
4. umount /cdrom

## Guest Addition
1. yum groupinstall “Development Tools”
- groupinstall 설치 중 에러가 발생하면 --skip-broken 옵션 추가
2. yum install -y make kernel-devel gcc perl
3. VBoxGuestAdditions_6.1.30.iso 파일 추가
4. 콘솔
- mount -r /dev/cdrom /media
- cd /media
- ./VBoxLinuxAdditions.run

다운로드 링크 : http://download.virtualbox.org/virtualbox/6.1.30

## Guest 확인
```
/sbin/rcvboxadd
lsmod | grep -i vbox

mount -t vboxsf 호스트설정_공유폴더명 게스트설정_마운트폴더명
```

## 에러
내용 : VirtualBox Additions module not loaded!
해결 : 확장팩 설치시 gcc같은 툴이 필요한데 설치되지 않아서 생기는 에러

```
yum groupinstall "Development Tools"
yum install kernel-headers kernel-devel
```