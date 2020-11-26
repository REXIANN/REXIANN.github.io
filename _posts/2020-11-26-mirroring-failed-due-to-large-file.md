---
layout: post
title: "100메가가 넘는 파일로 인해 gitlab의 저장소를 github에 미러링이 불가능 할때"
tags: [mirroring, git lfs, bfg, github, gitlab]
excerpt_separator: <!--more-->

---

## 사건의 발단

마지막 프로젝트를 gitlab에서 진행하였다. 프로젝트를 위해 MySQL에 넣을 상당한 크기의 데이터셋 역시 backend 폴더에 저장이 되어 있는 상태였고 완성된 데이터셋을 집어넣고 DB와 back, front 를 연동해가면서 프로젝트를 마무리했다. 이제 나의 결과물을 github에 있는 내 리포지토리로 옮겨야지! 하고 미러링을 시도한 순간, 에러가 생겼다..  

<!--more-->

## 문제의 원인

~~이미 커밋을 찍었어   롤백은 절대안돼   git lfs 하기엔 이미 늦었어   그럼 어떡해..?~~

문제는 다름아닌 우리가 사용하던 데이터셋에 있었다. github에는 기본적으로 100MB가 넘는 파일을 올릴 수 없게 되어 있는데 우리가 사용한 데이터셋은 가장 큰 파일의 경우 120MB에 육박했다. 파일 네 개의 크기가 도합 200MB가 넘는데... 부랴부랴 마스터 브랜치에서 데이터셋을 삭제하고 다시 시도해 보았지만 이미 로그에 데이터셋들이 남아있는 한 github으로의 미러링 시도는 계속 빨간 불만 들어왔다.

데이터셋이 탑재된 채로 얼마나 커밋이 찍혔는지 보았더니 대략 2주 정도였다. 2주치 작업량을 몽땅 날리기 위해 롤백을 할 수는 없었다. 찾아보니 `git lfs` 라는 git에서 사용되는 큰 파일(100MB이상)들을 잘게 분해하여 올려주는 툴도 있었지만 그걸 사용하기엔 이미 늦어버린 상태. 포기하기 않고 솔루션을 찾아 헤맸다.

## 트러블슈팅

먼저 날 구원해 준 감사한 사이트를 소개한다

[BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)

이 사이트의 소개를 읽어보자

> The BFG is a simpler, faster alternative to `[git-filter-branch](http://git-scm.com/docs/git-filter-branch)` for cleansing bad data out of your Git repository history: Removing Crazy Big Files, Removing Passwords, Credentials & other Private data

Removing Crazy Big Files!!!!! 바로 이거다. 여기서 제공하는 bfg.jar 파일을 통해 커밋 로그에 남아있는 엄청나게 큰 파일들을 지울 수 있다고 한다. 사용법을 찬찬히 읽다가 한 문장이 눈에 들어온다.

```bash
$ java -jar bfg.jar --strip-blobs-bigger-than 100M some-big-repo.git
```

그렇다. 저걸 사용하려면 자바가 필요하다. 오우노우. 설치를 위해 또다시 구글링을 했다.

원칙적으로 brew를 통한 openjdk 설치는 불가능하지만 AdoptOpenJDK라는 커뮤니티가 있었다. 이 커뮤니티는 prebuild 형태로 java binary를 제공한다고 한다. 공식적으로 OpenJDK를 직접 빌드해서 사용할 수도 있지만 자잘한 설정 문제(JAVA_HOME, 버전 업그레이드 등)가 있다고 해서 AdoptOpenJDK를 사용해서 설치하기로 했다. 사이트에서는 OpenJDK8부터 15까지 있었는데 bfg를 사용하기 위해서는 최소 8버전 이상이 필요하다고 한다. 나는 그냥 10버전을 설치했다.

```bash
$ brew tap AdoptOpenJDK/openjdk
$ brew cask install adoptopenjdk10

==> Verifying SHA-256 checksum for Cask 'adoptopenjdk10'.
==> Installing Cask adoptopenjdk10
Password:
🍺  adoptopenjdk10 was successfully installed!
```

중간에 암호입력이 있으니 노트북의 암호를 한번 입력해주도록 하자.

그리고 BFG사이트에서 .jar 파일을 다운받아야 한다. 여기서 받을 수 있다.

![img/Screen_Shot_2020-11-26_at_9.18.11_PM.png](/assets/img/posts/2020-11-26-mirroring-failed-due-to-large-file/Screen_Shot_2020-11-26_at_9.18.11_PM.png)

이제 준비가 끝났다. 먼저 해당 레포지토리를 미러링해와야 한다. 클론 해 올때 ❗반드시 `--mirror` 옵션을 붙여주어야 한다.

```bash
$ mkdir bfg-test
$ cd bfg-test
$ git clone --mirror your-repository.git
```

그리고 클론해온 .git 폴더가 있는 곳에 다운받은 .jar 파일을 넣어주자. 내가 받은 .jar 파일은 bfg-1.13.0.jar 이었기에 이후의 명령어는 전부 저걸 사용한다. 다운로드 받은 버전이 다르다면 자신의 버전을 넣어주면 된다. 매킨토시의 경우 자바를 설치하고 .jar 파일을 사용하려는 과정에서 security문제로 자꾸 막는 경우가 있는데 환경설정 > 보안 탭에서 해당 프로그램의 실행을 허가해주면 된다. 

파일 두개가 다 있는지 확인해 보고 실행하자

```bash
$ ls
bfg-1.13.0.jar s03p31a504.git

$ java -jar bfg-1.13.0.jar --strip-blobs-bigger-than 100M s03p31a504.git

Scanning packfile for large blobs: 1865
Scanning packfile for large blobs completed in 40 ms.
Found 3 blob ids for large blobs - biggest=142105251 smallest=124806456...
...
Deleted files
-------------

	Filename               Git id
	---------------------------------------------------------------
	KMDB_Moviefinal.json | caf16cbf (122.4 MB), 6811aa70 (135.5 MB)
	KMDB_movie.json      | 9fbd20dc (119.0 MB)

BFG run is complete! When ready, run: git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

됐다. 보이는가 저 거대한 파일들이? 지난 로그에서 성공적으로 저 큰녀석들을 제거했다고 나온다. 이제 미러링된 깃으로 들어가서 다음 명령어를 실행해주면 된다.

```bash
$ cd s03p31a504.git
$ git reflog expire --expire=now --all && git gc --prune=now --aggressive

Enumerating objects: 1865, done.
Counting objects: 100% (1865/1865), done.
Delta compression using up to 16 threads
Compressing objects: 100% (1826/1826), done.
Writing objects: 100% (1865/1865), done.
Selecting bitmap commits: 227, done.
Building bitmaps: 100% (107/107), done.
Total 1865 (delta 1189), reused 576 (delta 0), pack-reused 0
```

여기까지 왔다면 거의 다 됐다. 이제 푸시만 하면 되는데 여기서 gitlab에 먼저 접속해서 해당 레포지토리의 브랜치들을 확인하자. 보호되고 있는 (protected가 걸려있는) 브랜치가 있다면 푸시가 되지 않는다. 

보호되는 브랜치에 의해 푸시가 막힘

```bash
➜  s03p31a504.git git:(master) git push
Enumerating objects: 62, done.
Counting objects: 100% (62/62), done.
Writing objects: 100% (490/490), 14.67 MiB | 1.88 MiB/s, done.
Total 490 (delta 62), reused 62 (delta 62), pack-reused 428
remote: Resolving deltas: 100% (313/313), completed with 24 local objects.
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
```

gitlab에서 protected를 해제해주고 다시 실행하니 정상적으로 동작한다.

```bash
➜  s03p31a504.git git:(master) git push
Enumerating objects: 62, done.
Counting objects: 100% (62/62), done.
Writing objects: 100% (490/490), 14.67 MiB | 999.00 KiB/s, done.
Total 490 (delta 62), reused 62 (delta 62), pack-reused 428
remote: Resolving deltas: 100% (313/313), completed with 24 local objects.
...
+ c843587...94d827b refs/merge-requests/80/head -> refs/merge-requests/80/head (forced update)
+ 66bd8b6...5d7a856 refs/merge-requests/81/head -> refs/merge-requests/81/head (forced update)
+ e358148...c8cc0e0 refs/merge-requests/82/head -> refs/merge-requests/82/head (forced update)
```

## 결과

이제 gitlab에 있는 나의 레포지토리로 가서 연동이 되는지 확인해보자. 설정 > 저장소 > 미러링 저장소 에서 확인할 수 있다. 

![img/Screen_Shot_2020-11-26_at_9.32.19_PM.png](/assets/img/posts/2020-11-26-mirroring-failed-due-to-large-file/Screen_Shot_2020-11-26_at_9.32.19_PM.png)

나와 내 친구의 Github에 미러링이 다시 되는 모습을 확인할 수 있다. 나의 고통을 대변해 줄 지난 날들의 커밋들... 무사히 잔디의 일부분으로 만들었다.. 😭 쓰고보니 별 거 없지만 여기에 나의 저녁시간을 다 부었다는거... 크흑..

그래도 덕분에 1. Github에 100MB 넘는 파일은 업로드 되지 않는다는 점 2. `git lfs` 를 통해 이를 사전에 예방할 수 있다는 점 3. 여러 제약 조건의 차이로 인해 gitlab과 github의 연동이 방해받을 수 있다는 점 4. 이미 커밋을 찍어버렸고 되돌릴 수 없다면 지난 로그에서 지워줄 수 있다는 점 을 알 수 있었다. 오늘도 이렇게 하나 배워간다!