---
layout: post
title: "Docker(2)"
tags: [docker, dockerfile, dockerfile commands]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0

---

## Dockerfile로 이미지 만들기!

<!--more-->

도커 이미지를 추가하는 방법은 크게 세 가지가 있다. 먼저 `pull`을 사용해 미리 만들어져 있는 이미지를 가져오는 방법이 있다. 두번째 방법은 컨테이너의 변경사항으로부터 이미지를 만드는 방법이다. 세번째는 `Dockerfile`을 빌드하는 방법이다. `Dockerfile`은 도커만의 특별한 DSL로 이미지를 정의하는 파일이다. 



```bash
$ docker build -t ubuntu:{file-name}
```

웹 어플리케이션 서버를 실행하기 위한 도커 이미지를 만들어보자. 어플리케이션 실행을 위해 도커 이미지를 만드는 작업은 Dockerizing(도커라이징)이라고 한다. 도커파일에 적히는 명령어들은 `ADD`, `COPY`, `FROM`, `RUN`, `ENV`, `WORKDIR`, `EXPOSE`, `CMD` 등이 있다. 

* `ADD`: 빌드 중 호스트의 디렉토리에서 파일을 가져와서 이미지에 파일을 더하는 것이다. 이 때 빌드되는 디렉토리 밖에 위치하는 파일들은 가져오지 않으니 주의하자.
* `COPY`: `ADD`와 동일한 기능을 하나 압축파일을 자동으로 풀어주지 않는다.
* `CMD`: 컨테이너에서 실행될 명령어를 지정해준다.
* `FROM`: 어떤 이미지로부터 새로운 이미지를 생성할 지를 지정한다. `:`뒤에 특정 버전을 지정해서 사용할 수 있다.
* `ENTRYPOINT`: 컨테이너를 실행했을 때 실행할 명령을 적는다. 컨테이너를 정지했다가 다시시작해도 실행된다. 컨테이너가 실행될 때 실행되는 명령어이므로 도커 파일에서 한번만 사용할 수 있다.
* `ENV`: 컨테이너 실행 환경에 적용되는 환경변수의 기본값을 지정하는 지시자이다. 리눅스에서는 환경변수로 어플리케이션의 동작을 제어하는 경우가 자주 있는데 도커에서는 이러한 방식을 권장하는 편이며 직접 어플리케이션을 작성하는 경우에도 환경변수로 설정값을 넘겨받아 처리할 수 있도록 코딩해야 한다.
* `EXPOSE`: 가상머신에 오픈할 포트를 지정해준다. 컨테이너 외부에 노출할 포트를 지정해주며 컨테이너 실행 시 `-p` 옵션을 통해 연결해 주어야 한다.
* `LABEL`: 이미지에 라벨을 단다.
* `RUN`: 직접 명령어를 실행하는 지시자이다. 내려받은 이미지에 설치할 패키지 또는 쉘 명령어를 입력할 수 있다. RUN 바로 뒤에 적는 명령어가 실행된다. `sudo apt install` 등을 적을 수 있다.
* `USER`: 해당 이미지를 실행할 유저를 지정한다.
* `VOLUME`: 호스트의 디렉토리를 docker 컨테이너에 연결하는 명령어이다. 여러가지 설정파일, 데이터 등을 docker 컨테이너에서 사용할 수 있게 해준다. 즉 디렉토리 내용을 컨테이너에 저장하지 않고 호스트에 저장하도록 설정하는것이다. 주로 로그 수집과 같은 데이터 저장에 쓰인다.
* `WORKDIR`: 이후에 실행되는 작업의 실행 디렉터리를 변경한다. 리눅스의 `cd` 라고 생각하면 된다. `RUN` 명령어에 적는 cd 명령어는 해당 명령어가 실행되는 위치만 바꿔줄 뿐 디렉터리의 이동은 일어나지 않기 때문에 특정 디렉터리로 이동해서 명령을 수행할 때에는 `WORKDIR`이 훨씬 유용하다.
* `\`: 해당 명령이 이번 줄에서 끝나지 않고 다음 줄에 이어진다는 것을 의미한다. 커맨드라인이 길어질 경우 줄의 맨 끝에 붙여 주어야 한다.

## `docker run` 과 관련된 예약어

* `-d`: 컨테이너를 백그라운드에서 실행한다.
* `-p`: 포트포워딩을 지정하는 옵션이다. `:` 을 경계로 앞에는 외부 포트, 뒤에는 컨테이너 내부 포트를 지정할 수 있다. `9999:80`이라고 쓰면 9999로 들어오는 연결을 컨테이너에서 실행된 서버의 80포트로 보내겠다는 의미이다. 
* `-i`:컨테이너를 포어그라운드에서 실행한다.