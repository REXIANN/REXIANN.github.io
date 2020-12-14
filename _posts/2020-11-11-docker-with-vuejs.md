---
layout: post
title: "Docker를 이용한 Vuejs 구동"
tags: [docker, vue]
excerpt_separator: <!--more-->

---

## 기본환경 세팅!

윈도우 도커를 사용했으며 터미널에도 미리 도커가 설치된 상태이다.

<!--more-->

도커는 리눅스 기반이기 때문에 윈도우에서 도커를 사용하게 되면 결국은 VM을 사용해서 도커를 돌리는 셈이라지만 그래도 로컬에서 충분히 지지고 볶아가며 연습을 해야하기 때문에 상관하지 않기로 했다. 

윈도우 도커를 설치하고 터미널에도 미리 도커가 설치된 상태이다. (터미널을 통한 도커설치는 이전 포스트를 참고!) 이제 `frontend` 폴더로 이동했다. 폴더의 구조는 다음과 같다.



```bash
 ─frontend
    ├─node_modules
    ├─public
    └─src
        ├─api
        ├─assets
        │  ├─css
        │  └─img
        ├─components
        ├─plugins
        ├─views
        └─vuex
```


Vue를 구동하기 위해서는 node 의 환경이 필요하다. 내 경우 로컬에 설치된 노드의 버전이 12.18 이었으니 도커허브에 있는 노드 이미지도 동일한 버전을 가져오자. 

```bash
$ docker pull node:12.18
```

이제 도커파일을 만들어주자.

```bash
$ cd frontend
$ touch Dockerfile
```

도커파일에 작성한 내용은 다음과 같다.

```dockerfile
FROM node:12.18
WORKDIR ~/Desktop/s03p31a504/frontend

COPY package.json .

ADD . .
RUN npm install
CMD ["npm", "run", "serve"]
```

먼저 받아온 노드 이미지를 확장해야 하므로 `FROM` 명령어를 통해 해당 이미지를 가져온다. `WORKDIR` 은 해당 폴더가 있는 위치를 잡아준다. `package.json`에 있는 내용을 가져와서 사용해야 하므로 해당 파일을 `COPY`하고 현재 폴더에 있는 모든 폴더와 파일을 `ADD`를 통해 가져온다. 그 다음 `package.json`에 있는 관련 라이브러리들을 설치하는 명령어 `npm install `을 적어주었다. 그 뒤 최종적으로 실행할 커맨드 `npm run serve` 를 적어준다.

이제 도커파일을 빌드하자

```bash
$ docker build -t node:frontend .
```

터미널을 통해 도커가 도커파일에 적힌 명령어들을 차례대로 빌드하는 모습을 볼 수 있다.

```bash
Step 1/6 : FROM node:12.18
 ---> 28faf336034d
Step 2/6 : WORKDIR ~/Desktop/s03p31a504/frontend
 ---> Running in b3b45bcce7d7
Removing intermediate container b3b45bcce7d7
 ---> a9331fe771f4
Step 3/6 : COPY package.json .
 ---> d4b778f3bd56
Step 4/6 : ADD . .
 ---> 81788a4b6faf
Step 5/6 : RUN npm install
 ---> Running in fa54cc7a28b9

audited 1487 packages in 6.666s

65 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities

Removing intermediate container fa54cc7a28b9
 ---> fad5cf7fcf04
Step 6/6 : CMD ["npm", "run", "serve"]
 ---> Running in 038805fca056
Removing intermediate container 038805fca056
 ---> 31168883c2b3
Successfully built 31168883c2b3
Successfully tagged node:frontend
```

이제 설치한 도커 앱을 보면 frontend 태그를 가진 node 가 생성되어 있음을 볼 수 있다. 

![image-20201111220321597](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20201111220321597.png)

실행하는 방법은 다음과 같다

```bash
$ docker run -d -p 3333:3000 --name test1 node:frontendtest
```

이름은 그냥 test1으로 지정해주고 외부포트 3333번으로 들어올 때 내부포트 3000번으로 연결이 되도록 설정했다. 내 경우 vuejs 에서 내부적으로 3000포트를 사용하도록 해놓았기에 이렇게 설정했다. 

![image-20201111235339150](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20201111235339150.png)

잘 작동하는 것을 볼 수 있다. 이제 nginx 를 사용해서 배포까지 이어서 달려보자.