---
layout: post
title: "Jekyll을 활용한 나만의 Github Pages만들기"
tags: [Jekyll, Github, GithubPages, Ruby, gem, blog]
excerpt_separator: <!--more-->

---

## Github과 함께라면 나만의 웹페이지를 공짜로 만들 수 있다고?

어느 직업이든 마찬가지겠지만, 한 업계에 대해 공부하거나 일을 하다 보면 필수적인 기술 스택 뿐만 아니라 업계의 근황이나 최근 트렌드, 알면 유용한 이런저런 꿀팁들이 돌고 돌기 마련이다.<!--more--> 나는 웹 개발을 주로 하고 있는데 개발을 하면서  그동안 내가 배우고 익힌 것들을 마크다운으로 정리를 해 놓았었는데, 최근 이 글들을 어떻게 활용할까 하다가 예전에 한창 유행했던  Github Pages를 이용해서 나만의 블로그를 만들기로 했다. 
많은 개발자 분들이  자신의 포트폴리오와 사용가능한 기술, 했던 프로젝트를 정리해서 이력서처럼 만든 정적인 웹사이트를 쓰는 것도 보았기에 이참에 나도 나만의 글쓰기 공간 + 포트폴리오가 될 수 있는 나만의 개인 블로그를 시작하기로 마음먹었다.  물론 서버와 도메인을 사서 꾸미는 방법도 있지만 Github Pages를 이용하면 무료로 나만의 개인 페이지를 만들 수 있다는 생각에 바로 찾아보았다. 

먼저 Github Pages를 만드는 방법은 생각보다 간단했다. 그저 내 계정에 하나의 레포지토리를 만들면 되는데 주의 할 점은 해당 레포지토리의 이름은 무조건 `내 아이디.github.io`가 되어야 한다는 것이었다. 내 경우에는 REXIANN.github.io가 레포지토리의 이름이 되어야만 했고 그렇게 만들었다.

![Screen Shot 2020-08-29 at 8.48.51 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 8.54.51 PM.png)





## Jekyll, 넌 누구니?

그리고 Github Pages는 보통 Jekyll을 사용해서 관리한다기에 Jekyll이 뭔지 찾아보았다. 위키피디아에 따르면 제킬은 간단하고, 블로그 위주의 정적 웹사이트 생성기라고 한다. *루비* 라는 언어로 쓰여졌으며 깃헙의 공동 창업자는  Tom Perston-Werner가 만들었다고 한다. 어쩐지 Jekyll공식 사이트에 가보면 메인화면에 GIthub Pages를 만드는데 아주 좋다고 크게 써놨더라니... 오픈소스 라이선스는 MIT License를 따른다고 한다. 

내가 원하는 건 Jekyll을 가지고 정적인 웹페이지를 만드는 것일뿐, 다른 설명은 필요하지 않다. 바로 넘어가자. Jekyll을 쓰려면 gem이라는 라이브러리가 필요한데 이 라이브러리는 아까 위키피디아에서 보았던 ruby라는 언어의 라이브러리 이다. django를 쓰기위해 파이썬 라이브러리 매니저인 pip가 필요하듯이 jekyll을 쓰기 위해서는 루비 라이브러리 매니저인 gem이 필요하다. 당장 ruby를 깔러 가자. 윈도우를 사용하는 유저라면 [윈도우에서 루비를 설치할 수 있는 사이트](https://rubyinstaller.org/) 로 가서 설치를 하면 되고 나는 맥을 사용하기에 iTerm을 열고 다음 명령어를 입력했다. (iterm과 brew사용이 낯선 맥유저라면 먼저 맥 터미널 설정에 대한 다른 글들을 보고 오시는걸 추천합니다! 전 oh-my-zsh + iterm + brew를 사용중입니다.)

```bash
$ brew install ruby
```

설치가 되었다면 터미널에 gem을 입력해보자. 아마 gem 과 붙여서 사용할 수 있는 다른 옵션과 명령어에 대한 설명이 나온다면 정상적으로 루비가 설치된 것이다.

![Screen Shot 2020-08-29 at 9.12.51 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 9.12.51 PM.png)

한마디로 **Jekyll**은 ruby로 만들어진 라이브러리이며 gem에 의해서 관리된다고 볼 수 있다. 그럼 이제 gem을 이용해서 Jekyll을 설치하면 된다!

```bash
$ gem install bundler jekyll
```

다음과 같이 명령어를 치면. jekyll과 bundler라는 라이브러리가 설치된다. 그럼 이제 jekyll을 이용해서 아주 쉽고 빠르게 정적인 웹사이트를 만들 수 있다. 터미널의 위치를 적당한 곳에 옮긴다. 나는 Desktop에 jekylls라는 폴더를 만들어서 했다. 우선 현재위치를 pwd로 찍어보고 맞다면 다음 명령어를 실행한다.

```bash
$ jekyll new my-awesome-site
```

그러면 해당 폴더에 my-awesome-site라는 폴더가 생겼을 것이다. 그 안으로 우선 들어가보자.

```bash
$ cd my-awesome-site
```

해당폴더엔 다음과 같은 파일이 있다.

![Screen Shot 2020-08-29 at 9.57.26 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 9.57.26 PM.png)

자 뭔지 모르겠지만 우선 `_posts` 로 가보자. 마크다운 파일이 하나 보일 것이다. 열어보면 일반 마크다운과는 조금 다르게 맨 위에 무언가가 쓰여져 있다.

![Screen Shot 2020-08-29 at 10.03.33 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.03.33 PM.png)

이건 가급적이면 지우지 말고 'title'과 'date'만 수정해가면서 쓰자. 물론 필요하다면 다른 것들도 수정하면 된다. 이어서 마크다운의 본문을 읽어보면 Jekyll의 사용법에 대해 설명해주고 있다. 읽어보면

* 지금 이 글은 `_posts` 디렉터리에 있다.(아무래도 새로운 게시글들도 여기에 넣으라는 것 같다). 
* `jekyll serve` 명령어를 통해 웹사이트를 빌드할 수 있으며 빌드 되고 실행 중인 상태에서도 수정이 가능하다.
* 게시글들은 `YEAR-MONTH-DAY-title.MARKUP` 이라는양식을 맞추어야 한다. `YEAR` 은 네자리 숫자여야 하며,  `MONTH` 와 `DAY` 는 두자리 숫자여야 한다. `MARKUP` 은 그냥 파일에 사용된 확장자라고 한다.

또한 Jekyll은 강력한 코드 스니펫을 제공한다. 

```jekyll
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}
```

정확하게는 모르겠지만 장고에서 쓰던 DTL(Django Template Language)와 유사한 패턴도 보이고, 파이썬에서 사용하는 함수 입력 같은 것이 보이니 어느정도 감은 온다. 일단은 그냥 넘어가자.

마크다운 파일을 닫고 다시 터미널로 돌아가자. `my-awesome-site` 폴더가 있는 공간에서 우리는 빌드라는 걸 해 보자. 빌드를 하게 되면 _posts에 있는 파일들을 Jekyll이 읽어서 웹사이트의 게시글이 되도록 만들어 줄 것이다. 다음 명령어를 실행하자.

```bash
$ bundle exec jekyll serve
```



`jekyll serve` 만 사용해도 빌드에는 문제없지만 Jekyll 공식 홈페이지에 이렇게 나와있는 것으로 보아 bundler를 통해서 다른 번들도 같이 실행시켜주나보다. 빌드는 오래 걸리지 않는다. 내 경우에는 0.524초가 걸렸다.

![Screen Shot 2020-08-29 at 10.18.15 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.18.15 PM.png)

밑에 보이는 Server address를 Ctrl(cmd) + 클릭 하자. 다음과 같은 웹사이트가 보인다면 이미 당신의 웹사이트가 만들어진 것이다.![Screen Shot 2020-08-29 at 10.19.34 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.19.34 PM.png)

들어가 보면 우리가 아까 보던 마크다운 파일이 나와있다. 혹시 그 마크다운 파일명이 기억나는가? 여기서 중요한건 아까 우리가 열어본 마크다운 파일의 이름은 `2020-08-29-welcome-to-jekyll` 이었다는 것이다. 그러나 지금 우리가 보고 있는 게시글의 제목은 Welcome to Jekyll!이다. 사실 파일이름은 아무 상관이 없다. 아까 내가 지우지 말고 수정만 하라고 했던 그 레이아웃을 다시 보자. title과 date가 웹사이트를 구현할 때 반영이 된 것을 볼 수 있다. 

그럼 왜 파일이름을 그렇게 지으라고 했을까? 다시 탐색기 또는 파인더를 열어 보자.

![Screen Shot 2020-08-29 at 10.23.52 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.23.52 PM.png)

아까와 다르게 `_site`라는 폴더가 생겼다! 아까 `bundle exec jekyll serve`라는 명령어를 통해 빌드되면서 생긴 폴더가 바로 _site인 것 같다. 들어가보면 또 여러개의 폴더와 파일들이 있는데 중요한건 우리가 보고 있는 웹사이트는 실질적으로 _site 폴더 안에 있는 index.html이라는 점만 기억하면 될 것 같다. 또한 jekyll 폴더를 들어가보면 폴더가 계속 있다. 설명보다는 한번 보면 이해가 빠를 것 같으니 우선 파인더의 계층보기 뷰를 통해 보자.![Screen Shot 2020-08-29 at 10.27.31 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.27.31 PM.png)

어디서 많이 보던 구조 아닌가? 2020 다음에 08, 다음에 29, 그리고 `welcome-to-jekyll.html`이라는 저 파일 구조?

그렇다. `2020-08-29-welcome-to-jekyll` 마크다운 파일이 앞의 년,월,일에 따라 다른 폴더에 정렬되고 `welcome-to-jekyll.html`이라는 웹페이지를 구성하는 .html파일로 변환되어 있는 것을 볼 수 있다. 우리가 작성한 글의 제목이 이런 규칙에 따라 폴더에 나뉘어 담기고 빌드를 통해 마크다운 파일을 .html로 만들어주는 것이 `jekyll serve`의 일임을 알 수 있었다. 이제 남은건 Github Pages에 올려서 내가 Jekyll로 만든 웹사이트가 Github에서 돌아가면 끝이다. 

## 무엇을 Github와 이어야 하는가?

중요한 건 내가 만든 `my-awesome-site` 폴더 자체를 Github에 올리는 게 아니다! 빌드의 결과물로 나오는 `_site` 폴더가 github에 올라가야 하는 것이 오늘의 마지막 중요 포인트이다. 

우선 _site 폴더로 이동해서 깃을 시작하자

```bash
$ git init # git을 통한 버전 추적 시작
$ git add --all # 해당 폴더에 있는 파일과 하위폴더들을 전부 깃에 추가.(staged)
$ git commit -m "Initial Commit" # 커밋메시지 작성과 함께 커밋.
```

그리고 해당 깃을 내가 만든 `USERNAME.github.io` 의 원격저장소와 이어주어야 한다. 방법은 다음과 같다.

```bash
$ git remote add origin {`USERNAME.github.io`의 깃허브 주소}
$ git push -u origin master
```

레포지토리에도 나와있으니 그래도 복사-붙여넣기를 해도 무방하다. 푸시에 성공했다면 해당 레포지토리의 settings에 가보자.

스크롤을 내리다 보면 Github Pages라는 항목이 보일 것이다. 우리가 손쓸 것도 없이 이미 배포가 되고 있음을 볼 수 있다.

![Screen Shot 2020-08-29 at 10.38.32 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.38.32 PM.png)

이제 남은건 저 주소를 클릭하여 나의 블로그에 들어가 실제로 내가 만든 글이 있나 보면된다. 역시나, 반가운 Welcome to Jekyll!이라는 글이 날 반긴다.

그리고 기본적인 Jekyll이 단조롭게 느껴진다면 http://jekyllthemes.org/ 라는 사이트에 들어가보자. 많은 사람들이 만들어놓은 템플릿들이 있으니 그중에 원하는 것을 찾아 적용하면 된다. 단, 템플릿마다 조금씩 구조가 다르므로 구조에 대한 이해를 하기 위해서 다운받은 테마를 로컬에서 `jekyll serve` 를 통해 빌드해가며 수정해보고 배포가 되는지 확인해보는 것을 추천한다. 나는 Type-on-Strap이라는 테마를 사용했다. ![Screen Shot 2020-08-29 at 10.42.12 PM](/assets/img/posts/2020-08-29-githubpages-with-jekyll/Screen Shot 2020-08-29 at 10.42.12 PM.png)

이렇게 다양한 테마를 찾아보고 본인이 원하는 테마를 골라 잘 꾸며보는 것도 나쁘지 않은 것 같다. 물론 이제 나의 기록들을 채워넣으라 또 한동안 바쁘겠지만..

그럼 Jekyll을 이용하여 나만의 Github Pages 만들기 포스트는 여기까지!
