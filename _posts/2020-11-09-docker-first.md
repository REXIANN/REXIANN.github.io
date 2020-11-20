---
layout: post
title: "Docker(1)"
tags: [docker, image, container, docker hub, VCS]
excerpt_separator: <!--more-->

---


## 도커 설치

```bash
curl -s https://get.docker.com | sudo sh
```

설치하고 나면 확인해보자

<!--more-->

```bash
$ docker -v
# Docker version 19.03.13, build 4484c46d9d
$ cat /etc/apt/sources.list.d/docker.list
#cat: /etc/apt/sources.list.d/docker.list: No such file or directory
$ dpkg --get-selections | grep docker
# docker-ce                                       install
# docker-ce-cli                                   install
```

`dpkg` 명령어로 설치된 패키지 중에 docker 이름이 들어간 패키지를 찾아보면 `docker-ce` 와 `docker-ce-cli` 가 설치된 것을 볼 수 있다. 

원래 도커 패키지는 `docker-engine` 하나였지만 현재는 `docker-ce`와 `docker-ce-cli`두개로 나뉘어져 있다. 여기서 `ce`는 커뮤니티 에디션의 줄임말이다. 패키지가 나눠져 있는 이유는 도커의 아키텍처를 이해해야 하는데, 도커는 도커 엔진과 도커 클라이언트로 나뉜다. 도커 엔진은 서버로 동작하며 시스템 상에 서비스로 등록된다. 도커 클라이언트는 사용자가 입력하는  `docker` 명령어이다. 이 명령어를 실행하면 클라이언트는 도커 서버에 명령을 전달하고 명령은 서버에서 처리된다. 도커 클라이언트로 외부의 도커 서버에 명령을 내리는 것도 가능하다. 

도커를 설치하면 도커 엔진의 시스템 서비스도 같이 실행된다. 클라이언트는 이 서비스 프로세스에 명령어를 보내는 방식으로 작동한다. 이제 도커 명령어를 하나 실행해보자. `docker ps`는 형재 실행중인 모든 컨테이너 목록을 출력하라는 명령어이다. 

```bash
$ docker ps
# Got permission denied while trying to connect blabla
```

도커 데몬에 연결할 수 없다는 에러 메시지가 나온다. 에러가 발생하는 이유는 사용자에게 도커 소켓에 접근할 권한이 없기 때문이다. 관리자 권한이 있다면 명령어 앞에 `sudo`를 붙여서 실행할 수 있다. 사용자 계정에서도 도커를 직접 사용할 수 있도록 `docker` 그룹에 사용자를 추가해주자. 이때 관리자 권한이 필요하다.

```bash
$ sudo usermod -aG docker $USER
$ sudo su - $USER
```

이젠 `sudo` 없이도 도커 명령어를 사용할 수 있다.

```bash
ubuntu@ip-172-26-13-183:~$ docker ps
# CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

```

## Docker image

도커를 이해하기 위해서는 이미지와 컨테이너에 대해 알아야 한다. 이미지는 가상머신에서 사용하는 이미지와 비슷한 역할로, 어떤 애플리케이션을 실행하기 위한 환경이라 할 수 있다. 그리고 이 환경을 파일들의 집합으로 이루어져 있다. 도커는 어플리케이션을 실행하기 위한 파일들을 모아놓고 어플리케이션과 함께 이미지로 만들 수 있다. 그리고 이 이미지를 기반으로 어플리케이션을 바로 배포할 수 있다. 

이미지 다운로드는 `pull` 명령어를 통해 도커 레지스트리에서 받아오는 방식으로 사용 가능 하다. 도커에서는 이미지 다운로드, 업로드, 생성 시 각각 `pull`, `push`, `commit` 의 명령어를 사용한다. 깃의 명령어와 똑같다는 걸 눈치챘다면 당신은 이미 진정한 개발자! 도커의 개발자들은 명령어를 정할 때 개발자들을 배려해서 정한 듯 하다.

```bash
$ docker pull hello-world
# hello-world 이미지를 다운로드
$ docker images
# REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
# hello-world         latest              bf756fb1ae65        10 months ago       13.3kB
```



## Docker Hub - 도커 공식 이미지 레지스트리

도커에서는 도커허브라는 이미지 호스팅 서비스를 공식적으로 제공한다. `docker info`를 통해 클라이언트에 지정도니 기본 레지스트리의 주소를 확인할 수 있다.

```bash
$ docker info | grep Registry
# Registry: https://index.docker.io/v1/
```

도커 허브에는 아주 많은 이미지들이 등록되어 있는데 이 이미지들은 도커 사에서 공식적으로 제공하는 이미지와 사용자들이 직접 만들어 올린 이미지들로 나눠진다. 도커사에서는 Ubuntu, CentOS 같은 리눅스 운영체제 이미지와 MySQL, Redis 등 자주 사용되는 어플리케이션에 대한 공식 이미지를 제공하고 있다. 

일반적으로 도커가 제공하는 공식 이미지에는 네임스페이스가 없다. 네임스페이스는 이미지 이름에서 슬래시(`/`) 로 구분되며 슬래시 앞의 부분이 네임스페이스가 되고 슬래시 뒷부분이 실제 이미지 네임이 된다. 도커 공식 저장소에서는 사용자이름을 네임스페이스로 사용한다. 

## Docker Container - 격리된 환경에서 실행되는 프로세스

이미지는 어떤 환경이 구성되어 있는 상태를 저장해놓은 파일들의 집합이다. 이 이미지의 환경 위에서 특정한 프로세스를 격리시켜 실행한 것을 컨테이너라고 부른다. 컨테이너를 실행하려면 반드시 이미지가 있어야 한다. 이미지는 보조 기억장치에 저장되어 있는 프로그램이고 컨테이너는 보조 기억장치에서 읽어와 주기억장치에 로드되어 실행되는 프로세스 라고 볼 수 있겠다. 

컨테이너는 `docker run` 을 통해서 실행할 수 있다. 

```bash
$ docker run hello-world
# Hello from Docker!
# This message shows that your installation appears to be working correctly.
```

컨테이너를 조작할 때에는 컨테이너 아이디를 사용하거나 이름을 사용할 수 있다. 이름은 컨테이너를 실행할 때 `--name` 옵션을 사용해 지정할 수 있지만 지정하지 않으면 임의의 이름이 자동으로 부여된다. 보통 공식적으로 제공되는 이미지들은 명령어 기본값이 지정되어 있으니 참고하자. 해당 어플리케이션을 실행하는 명령어나 스크립트가 지정되어 있다. 

컨테이너는 독립된 환경에서 실행되며 컨테이너의 기본적인 역할은 이미지 위에서 미리 규정된 명령어를 실행하는 일이다. 이 명령어가 종료되면 컨테이너도 종료 상태에 들어간다. 종료 상태에 있는 컨테이너의 목록도 확인하려면 `docker ps -a`로 확인해보자.

```bash
# CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
# 374d0e363c31        hello-world         "/hello"            6 minutes ago       Exited (0) 6 minutes ago                       distracted_chebyshev
```

## 도커의 버전 관리 시스템

도커에서 이미지는 불변(Immutable)한 저장 매체이다. 그 대신 도커에서 이 이미지 위에 무언가를 더해서 새로운 이미지를 만들어내는 일은 가능하다. 이미지를 기반으로 만들어진 컨테이너가 가변(mutable)하기 때문이다. 

도커의 또 다른 중요한 특징은 바로 계층화된 파일 시스템을 사용한다는 점이다.  특정한 이미지로부터 생성된 컨테이너에 어떠한 변경사항을 더하고(파일을 변경하고), 이 변경된 상태를 새로운 이미지로 만들어내는 것이 가능하다. 도커의 모든 이미지는 기본적으로 이 원리로 만들어진다. 

### 연습
```bash
$ docker run -it --rm centos:latest bash
# centos:latest이미지로부터 bash 를 실행하라는 뜻
# 아직 이미지가 없다면 도커의 공식 저장소에서 이미지를 pull 해온다
# 그리고 해당 이미지를 기반으로 bash 프로세스를 실행한다
```
