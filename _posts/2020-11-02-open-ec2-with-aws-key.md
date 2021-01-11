---
layout: post
title: "AWS key(.pem 파일)를 이용하여 사이트 접속"
tags: [aws, pem]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0

---

우리가 운영본부로부터 받은 키(`K3A504T.pem` 파일)만 가지고 알아서 웹 서버에 접속해 프로젝트를 배포해야 한다! <!--more-->우리 사이트의 주소는 `ubuntu@k3a504.p.ssafy.io` 이고 사용하게 될 운영체제는 `ubuntu` 이므로 다음과 같이 Secure Shell로 접속을 하자.



```bash
$ ssh -i K3A504T.pem ubuntu@k3a504.p.ssafy.io
```

다음과 같은 메시지가 나온다. 

```bash
ECDSA key fingerprint is SHA256:thisissecret/abcdefghijklmnop.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

`yes` 를 눌러 진행하자.

```bash
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-1021-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Nov 10 05:47:33 UTC 2020

  System load:  0.0                Processes:           116
  Usage of /:   0.7% of 310.15GB   Users logged in:     0
  Memory usage: 2%                 IP address for eth0: 172.0.0.0
  Swap usage:   0%
```

짜잔! 우분투가 나왔다. 우분투를 사용하기 위해 기본적인 툴들을 설치하자

```bash
# 시스템 업데이트
$ sudo apt upgrade
$ sudo apt update
# 필수적인 패키지 설치
$ sudo apt install build-essential
$ sudo apt install python3 python3-pip
# pip3 upgrade
$ sudo pip3 install --upgrade pip
# 퍼블릭 키 발급받기
$ ssh-keygen -t rsa
# 엔터 -> 비밀번호 입력(건너뛰기) -> 키 불러오기
cat /home/ubuntu/.ssh/id_rsa.pub
# 깃의 퍼블릭 키 세팅시 deploy keys 붙여넣기
# 배포할 때 pull 받아서 비밀번호 입력하고 배포하면 됨
$ git clone {ssh-address-of-git}
# 백엔드 디렉토리로 이동
$ sudo apt install virtualenv
$ virtualenv -p python3 venv
$ source /venv/bin/activate
$ pip install -r requirements.txt
```

