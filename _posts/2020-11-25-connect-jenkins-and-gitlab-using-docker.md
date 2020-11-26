---
layout: post
title: "도커로 젠킨스 설치 및 깃랩과 연동"
tags: [test case, BDD, TDD]
excerpt_separator: <!--more-->


---

<!--more-->

## 젠킨스 설치

먼저 젠킨스 이미지를 내려받는다

```bash
$ sudo docker pull jenkins/jenkins:lts
```

그 후 젠킨스 이미지를 실행한다(컨테이너화)

```bash
$ docker run -d -p 8181:8080 -v /jenkins:/var/jenkins_home --name my_jenkins -u root jenkins/jenkins:lts
```

각 옵션의 뜻은 

- -d : 해당 컨테이너를 백그라운드에서 실행한다
- -p : 외부에서 접근할 포트: 접근시 연결되는 내부포트 를 지정(포워딩)해준다.
- -v: 호스트와 컨테이너의 디렉토리를 연결(마운트) 한다.
- —name: 실행될 컨테이너의 이름을 지정해준다
- -u: 실행할 사용자를 지정한다
- 맨 마지막의 jenkins/jenkins:lts 는 실행할 이미지의 이름이며 해당 이름의 이미지 파일이 없을 경우 도커허브에서 찾아서 땡겨오려고 시도하므로 주의하자⚠️

 

실행된 컨테이너를 다음 명령어로 볼 수 있다.

```bash
$ docker ps
```

이제 `http://사이트주소:8181`로 들어가서 젠킨스를 켜 보자. 아마 키를 입력 하라고 나올 것이다.

여기서 두가지 방법이 있는데 첫번째는 우리가 마운트한 폴더를 들어가는 것이다.  `-v` 옵션을 통해 현재 루트에 있는 jenkins라는 폴더가 컨테이너의 `/var/jenkins_home` 이라는 폴더와 연동된 상태이다. 그러므로 마운트 된 폴더에 들어가거나 실행되어 돌아가고 있는 젠킨스 컨테이너에 직접 들어가서 확인하는 방법 두가지가 있다.

1. 마운트된 폴더에 들어가서 확인하기

    ```bash
    cd /jenkins/secretes
    cat initialAdminPassword
    ```

2. 젠킨스 컨테이너 안으로 들어가서 확인하기

    ```bash
    sudo docker exec -it my_jenkins /bin/bash
    cd /var/jenkins_home
    cat initialAdminPassword
    ```

찾은 키를 입력해주면 플러그인 설치 화면이 나온다. 웬만하면 Install suggested plugins 를 눌러서 젠킨스가 추천하는 플러그인들을 설치해주자. 시간이 조금 걸린다.

유저설정까지 해주면 다음과 같이 젠킨스가 나온다.

![related_images/Screen_Shot_2020-11-26_at_5.10.51_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.10.51_PM.png)

## 젠킨스와 깃랩 연동하기

아직은 젠킨스 기본 플러그인만 설치된 상태이다. 여기에 두가지 플러그인을 더 추가해야 깃랩과의 연동이 가능하다. 

- gitlab
- publish over SSH

플러그인 설치는 Manage jenkins → manage plugins 에서 가능하다

![related_images/Screen_Shot_2020-11-26_at_5.13.50_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.13.50_PM.png)

available plugins 탭으로 이동하여 검색 후 설치할 수 있다. 해당 칸을 누르는 것이 아닌 체크박스에 체크를 하고 밑에 있는 install 버튼을 눌러야 하니 헷갈리지 말자.

![related_images/Screen_Shot_2020-11-26_at_5.15.18_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.15.18_PM.png)

![related_images/Screen_Shot_2020-11-26_at_5.17.03_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.17.03_PM.png)

이제 젠킨스 내에서 작업을 위한 item을 생성해야 한다. 메인 페이지에서 New item을 선택하고 적당히 아이템 이름을 지어준 뒤 Freestyle Project를 선택한 뒤 맨 밑에 있는 OK 를 누르자.

![related_images/Screen_Shot_2020-11-26_at_5.20.44_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.20.44_PM.png)

깃랩에서 젠킨스에 트리거를 전송하려면 젠킨스에서 제공하는 secret token이 필요하다. Build triggers 항목의 'Build when a change is pushed to Gitlab. URL: 블라블라' 를 체크하고 밑에 있는 Advanced 를 눌러 상세 설정에 들어가자. 현재 가려놓은 부분은 이 젠킨스 아이템의 URL인데 나중에 깃랩에서 연동할 때 저 주소도 같이 입력해야 한다. 기억해두자.

![related_images/Screen_Shot_2020-11-26_at_5.24.01_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.24.01_PM.png)

확장된 탭을 보면 secret token 이라는 항목이 있다. Generate를 눌러 토큰을 생성하자.

![related_images/Screen_Shot_2020-11-26_at_5.25.46_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.25.46_PM.png)

토큰을 복사해서 잘 저장한 뒤 반드시 ⚠️왼쪽 하단에 있는SAVE 버튼을 눌러야 깃랩 트리거 설정이 완료된다!!

이제 젠킨스에서 만든 토큰을 넣어주러 깃랩으로 이동하자. 깃랩에서 연동하고자 하는 저장소로 이동한뒤 settings > integration 항목으로 이동한다.

![related_images/Screen_Shot_2020-11-26_at_5.31.45_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.31.45_PM.png)

URL에는 아까 위에서 기억하라고 했던 우리의 아이템 URL 그리고 토큰값을 넣어주면 된다. Push events에는 적용할 브랜치를 적어주면 되는데 빈 값으로 두게 되면 모든 브랜치에 적용된다. 한 마디로 모든 브랜치에서 푸시 이벤트가 일어날 때 마다 젠킨스가 트리거 된다는 뜻이니 웬만하면  master 하나만 적어서 불필요한 푸시 이벤트를 발생시키지 않는 것이 좋다.

다 되었다면 조금 내리다 보면 있는 Add webhook 버튼을 눌러 추가하자.

![related_images/Screen_Shot_2020-11-26_at_5.34.18_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.34.18_PM.png)

추가했다면 Add webhook 버튼 밑에 생성된 웹훅이 보인다. 테스트를 위해 푸시 이벤트를 발생시켜 보자.

![related_images/Screen_Shot_2020-11-26_at_5.35.53_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.35.53_PM.png)

성공했다면 다음과 같은 메시지를 볼 수 있다.

![related_images/Screen_Shot_2020-11-26_at_5.37.36_PM.png](/assets/img/posts/2020-11-25-connect-jenkins-and-gitlab-using-docker/Screen_Shot_2020-11-26_at_5.37.36_PM.png)