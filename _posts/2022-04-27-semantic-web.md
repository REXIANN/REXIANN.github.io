---
layout: post
title: "시맨틱 웹(Semantic Web) 이란"
tags: [semantic, web, html5]
excerpt_separator: <!--more-->
sitemap:
  changefreq: daily
  priority: 1.0
---

<!--more-->

시맨틱 웹(Semantic Web)은 '의미론적인 웹' 이라는 뜻으로, 현재의 인터넷과 같은 분산 환경 에서 리소스(웹 문서, 각종 파일,
서비스
등)에 대한 정보와 자원 사이의 관계-의미 정보(Semanteme)를 기계(컴퓨터)가 처리할 수 있는 온톨로지 형태로 표현하고, 이를 자동화된
기계가 처리 하도록 하는 프레임워크 이자 기술 입니다.

웹의 창시자인 팀 버너스리가 1998년 제안 했고, 현재 W3C 에 의해 표준화 작업이 진행 중 입니다.

## 시맨틱 웹과 현재 웹의 차이

기존의 HTML 로 작성된 문서는 컴퓨터가 의미 정보를 해석할 수 있는 메타 데이터 보다는 사람의 눈으로 보기에 용이한 시각 정보에 대한 메타
데이터와 자연어로 기술된 문장으로 가득 차 있습니다.

예를 들어 `<em>바나나</em>는 <em>노란색</em>이다.` 라는 문장 에서 `<em>` 태그는 단지 바나나 라는 단어를 강조 하기 위해
사용 됩니다. 이 HTML 을 받아서 처리하는 기계(컴퓨터)는 바나나 라는 개념과 노란색 이라는 개념이 어떤 관계를 가지는지 해석할 수 없습니다.
단지
해당 태그로 둘러싸인 구절을 다르게 표시하여 시각적으로 강조를 할 뿐입니다. 게다가 바나나가 노란색 이라는 것을 서술하는 문장을 자연어로
작성 되었으며, 기계는 단순한 문자열로 해석하여 화면에 표시합니다.

시맨틱 웹은 XML 에 기반한 시맨틱마크업 언어를 기반으로 합니다. 가장 단순한 형태인 RDF
는 `<Subject, Predicate, Object>` 의 트리플 형태로 개념을 표현 합니다.

위의 예를 트리플로 표현 하면 `<urn:바나나, urn:색, urn:노랑>` 과 같이 표현할 수 있습니다. 이렇게 표현된 트리플을 컴퓨터가 해석
하여
`바나나` 라는 개념은 `노랑` 이라는 `색`을 가지고 있다, 라고 해석 하고 처리할 수 있게 됩니다.

## HTML5 에서의 시맨틱 웹

SEO 와 같은 마케팅 도구를 사용하여 검색엔진이 본인의 웹사이트를 검색하기 알맞은 구조로 웹사이트를 조정하기도 하는데, 이것은 기본적으로
검색엔진이 웹사이트 정보를 어떻게 수집 하는지 아는 것으로부터 시작됩니다.

검색엔진은 로봇(Robot) 이라는 프로그램을 이용해 매일 전세계의 웹사이트 정보를 수집합니다. 이것을 크롤링이라 하며, 검색엔진의 크롤러가 이를
수행합니다. 그리고 검색 사이트 이용자가 검색할 만한 키워드를 미리 예상하여 검색 키워드에 대응하는 인덱스(색인) 을 만들어 둡니다. 이것을 인덱싱
이라 하며 검색엔진의 인덱서가 이를 수행합니다.

인덱스를 생성할 때 사용되는 정보는 검색 로봇이 수집한 정보인데, 결국은 웹사이트의 HTML 코드입니다(구글은 js 를 통해 렌더링 된 html
도 수집한다고 합니다). 즉 검색엔진은 HTML 코드만으로 그 의미를 인지하여야 하는데 이때 시맨틱 요소(Semantic Element)를
해석하게 됩니다.

HTML 로 작성된 문서는 컴퓨터가 해석할 수 있는 메타데이터와 사람이 사용하는 자연어 문장이 뒤섞여 있습니다. 아래 코드를 보면 1행과 2행은
브라우저에서 동일한 외형을 갖습니다. 이는 h1 태그의 디폴트 스타일이 1행과 같기 때문입니다.

```html
<font size="6"><b>Hello World</b></font>
<h1>Hello World</h1>
```

1행의 요소는 의미론적으로 어떤 의미도 가지고 있지 않습니다. 즉, 의도가 명확치 않습니다. 개발자가 의도한 요소의 의미를 명확하게 나타내지 않고 다만
폰트 크기와 볼드체를 지정하는 메타데이터만을 브라우저에게 알리고 있습니다. 그러나 2행의 요소는 제목 중 가장 상위 레벨이라는 의미를 내포하고
있어서 개발자가 의도한 요소의 의미가 명확히 드러나고 있습니다. 이것은 코드의 가독성을 높이고 유지보수를 쉽게 합니다.

검색엔진은 대체로 h1 요소 내의 콘텐츠를 웹문서의 중요한 제목으로 인식하고 인덱스에 포함시킬 확률이 높습니다. 또한 사람도 h1 요소 내의
콘텐츠가 제목임을 인식할 수 있습니다. 시맨틱 요소로 구성되어 있는 웹페이지는 검색엔진에 보다 의미론적으로 문서 정보를 전달할 수 있고 검색엔진
또한 시맨틱 요소를 이용하여 보다 효과적인 크롤링과 인덱싱이 가능해졌습니다.

즉, 시맨틱 태그란 브라우저, 검색엔진, 개발자 모두에게 **콘텐츠의 의미를 명확히 설명** 하는 역할을 합니다.

## HTML5 에서 사용 가능한 시맨틱 태그

여기서는 편의상 컨텐츠 구획을 나누는 태그들만을 정리합니다.
더 많은 HTML 태그들의 정의를 보고
싶다면 [MDN 공식문서](https://developer.mozilla.org/ko/docs/Web/HTML/Element) 를 찾아보세요.

### `header`

`<header>` 요소는 페이지의 소개 및 탐색에 도움을 주는 컨텐츠를 나타냅니다.
주로 제목, 로고와 같은 소개 내용을 포함 하며 검색 폼, 작성자 이름 등의 요소도 포함할 수 있습니다.
그리고 다른 `header` 나 `footer` 가 자손으로 올 수 없습니다.

`<header>` 요소는 사실 구획 컨텐츠가 아니므로 개요에 구획을 생성하지는 않습니다. 대신 주위 구획의 제목(`<h1>` ~ `<h6>`
요소)을 감싸기
위한 요소입니다. 따라서 필수는 아닙니다.

보통 페이지는 header 와 main으로 이루어지는 것이 보통입니다. 하지만 그렇다고 해서 header 가 맨 위에만 쓰여야 하는 것은 아닙니다.

```html
<!-- page header -->
<header>
  <h1>Main Page Title</h1>
  <img src="mdn-logo-sm.png" alt="MDN logo" />
</header>

<!-- article header -->
<article>
  <header>
    <h2>The Planet Earth</h2>
    <p>
      Posted on Wednesday,
      <time datetime="2017-10-04">4 October 2017</time>
      by Jane Smith
    </p>
  </header>
  <p>
    We live on a planet that's blue and green, with so many things still unseen.
  </p>
  <p>
    <a href="https://janesmith.com/the-planet-earth/">Continue reading....</a>
  </p>
</article>
```

### `nav`

`<nav>` 요소는 탐색 구획 요소 라고도 불립니다. 우리가 보통 생각하는 네비게이션 바를 의미합니다.
문서의 부분 중 현재 페이지 내, 또는 다른 페이지로의 링크를 보여주는 구획을 나타냅니다.
메뉴, 목차, 색인에 쓰일 수 있습니다.

```html
<nav class="crumbs">
  <ol>
    <li class="crumb"><a href="#">Bikes</a></li>
    <li class="crumb"><a href="#">BMX</a></li>
    <li class="crumb">Jump Bike 3000</li>
  </ol>
</nav>
```

### `main`

`<main>` 요소는 문서 `<body>` 의 주요 컨텐츠를 나타냅니다. 주요 컨텐츠 영역은 문서의 핵심 주제나 앱의 핵심 기능에 직접적으로
연결되었거나 확장하는 컨텐츠로 이루어집니다.

`hidden` 속성 없이는 문서에 하나보다 많은 main 요소가 존재해서는 안됩니다. 다시 말해 `<main>` 요소의 컨텐츠는 문서의 유일한
내용이어야 합니다.
따라서 사이드바, 탐색 링크, 저작권 정보, 사이트 로고, 검색 폼 등 여러 문서에 걸쳐 반복되는 콘텐츠는 포함해선 안됩니다.

`<main>`
요소는 [main 랜드마크](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/main_role)
역할과 동일하게 행동합니다. 따라서 접근성 보조기술이 문서의 큰 구획을 찾고 이동할 때 쓰입니다.

```html
<main>
  <h1>Apples</h1>
  <p>The apple is the pomaceous fruit of the apple tree.</p>

  <article>
    <h2>Red Delicious</h2>
    <p>
      These bright red apples are the most common found in many supermarkets.
    </p>
    <p>...</p>
    <p>...</p>
  </article>

  <article>
    <h2>Granny Smith</h2>
    <p>These juicy, green apples make a great filling for apple pies.</p>
    <p>...</p>
    <p>...</p>
  </article>
</main>
```

### `section`

`<section>` 요소는 일반 구획 요소 라고도 불리며 HTML 문서의 독립적인 구획을 나타냅니다.

대개 각각의 `<section>` 을 식별할 수단이 필요하다. 주로 제목(`<h1>` ~ `<h6>`) 요소를 `<section>` 의 자식으로
포함하는 방법을 사용합니다. 따라서 매우 소수의 예를 제외하고는 항상 **제목** 이 있는 것이 일반적입니다.

요소의 컨텐츠를 따로 구분해야 할 필요가 있으면 `<article>` 요소를 사용합니다.
따라서 `<section>` 요소는 본문의 여러 내용(article)을 포함하는 공간을 의미하기도 합니다.

`<section>` 요소를 일반적인 컨테이너로 사용해서는 안되며 HTML 문서에서 해당 구획이 논리적으로 나타나야 하면 `<section>`
을 사용하는 것이 좋습니다.

```html
<h1>Choosing an Apple</h1>
<section>
  <h2>Introduction</h2>
  <p>
    This document provides a guide to help with the important task of choosing
    the correct Apple.
  </p>
</section>

<section>
  <h2>Criteria</h2>
  <p>
    There are many different criteria to be considered when choosing an Apple —
    size, color, firmness, sweetness, tartness...
  </p>
</section>
```

### `article`

`<article>` 요소는 문서, 페이지, 애플리케이션, 또는 사이트 안에서 독립적으로 구분해 배포하거나 재사용할 수 있는 구획을 나타냅니다.
대표적으로 게시판 글이나 블로그 글, 뉴스 기사, 제품 카드 등이 있습니다.

하나의 문서가 여러 개의 `<article>` 을 가질 수 있습니다. 또한 필요한 경우 `<article>` 안에 다른 `<article>`
이나 `<section>` 이 위치할 수 있습니다.

`<article>` 역시 각각의 `<article>` 을 식별할 수단이 필요하며, 주로 제목(`<h1>` ~ `<h6>`)
요소를 `<article>` 의 자식으로 포함하는 방법을 사용합니다.

`<article>` 요소가 중첩되어 있을 때, 안쪽에 있는 요소는 바깥쪽에 있는 요소와 관련된 글을 나타내야 합니다. 예를 들어 블로그 글의
댓글은 글을 나타내는 `<article>` 요소 안에 중첩한 `<article>` 로 나타낼 수 있습니다.

필요한 경우 `<article>` 요소의 작성일자와 시간은 `<time>` 요소의 `<datetime>`속성을 이용해 나타낼 수 있습니다.

```html
<article class="film_review">
  <header>
    <h2>Jurassic Park</h2>
  </header>
  <section class="main_review">
    <p>Dinos were great!</p>
  </section>
  <section class="user_reviews">
    <article class="user_review">
      <p>Way too scary for me.</p>
      <footer>
        <p>
          Posted on
          <time datetime="2015-05-16 19:00">May 16</time>
          by Lisa.
        </p>
      </footer>
    </article>
    <article class="user_review">
      <p>I agree, dinos are my favorite.</p>
      <footer>
        <p>
          Posted on
          <time datetime="2015-05-17 19:00">May 17</time>
          by Tom.
        </p>
      </footer>
    </article>
  </section>
  <footer>
    <p>
      Posted on
      <time datetime="2015-05-15 19:00">May 15</time>
      by Staff.
    </p>
  </footer>
</article>
```

### `footer`

`<footer>` 요소는 가장 가까운 구획 컨텐츠나 구획 루트의 푸터를 의미 합니다. `<footer>` 에는 일반적으로 구획의 작성자, 저작권
정보, 관련 문서 등의 내용을 담습니다.

따라서 `<footer>` 요소는 우리가 일반적으로 생각하는 웹사이트의 하단에 위치하는 푸터 이외에도 다양한 곳에서 쓰일 수 있습니다.

`<footer>` 요소는 `<header>` 요소의 자손이 될 수 없습니다.

```html
<article>
  <h1>How to be a wizard</h1>
  <ol>
    <li>Grow a long, majestic beard.</li>
    <li>Wear a tall, pointed hat.</li>
    <li>Have I mentioned the beard?</li>
  </ol>
  <footer>
    <p>© 2018 Gandalf</p>
  </footer>
</article>
```

### `aside`

`<aside>` 요소는 별도 구획 요소 라고도 불립니다. 해당 요소는 문서의 주요 내용과 간접적으로 연관된 부분을 나타냅니다.
주로 사이드바 또는 콜아웃 박스(말풍선처럼 특정 영역을 가리키는 박스)로 사용됩니다.

```html
<article>
  <p>디즈니 만화영화 <em>인어 공주</em>는 1989년 처음 개봉했습니다.</p>
  <aside>인어 공주는 첫 개봉 당시 8700만불의 흥행을 기록했습니다.</aside>
  <p>영화에 대한 정보...</p>
</article>
```

## Good Example - Instagram

인스타그램의 경우 시맨틱 웹이 굉장히 잘 적용되어 있습니다. 구글이나 페이스북 보다도 더욱 많은 곳에서 시맨틱 태그들이 활용되고 있어 인스타그램을 통해 시맨틱 태그의 활용사례를 알아보겠습니다.
~~반례를 보고싶다면 네이버를 가면 됩니다~~

![ig](/assets/img/posts/2022-04-27-semantic-web/1-ig-landing.png)

인스타그램의 피드를 보기위해서는 로그인을 해야 합니다. 이 페이지의 경우 다음과 같이 구성되어 있습니다.

![ig-signin](/assets/img/posts/2022-04-27-semantic-web/2-ig-signin-ppt.png)
페이지안에서의 body 에서 "보여지는 부분" 을 하나의 section 으로 설정 했습니다.

그리고 section 안에는 main 과 footer 로 구성되어 있습니다.

main 안에서 자체로 의미를 가지는 article 이 하나 있는 것을 볼 수 있습니다. 사실상 여기서는 회원가입 또는 로그인을 하기 위한 폼 하나가 전부이므로 main 안에 바로 article 이 존재합니다.

![ig-landing](/assets/img/posts/2022-04-27-semantic-web/3-ig-landing-ppt.png)

로그인을 해서 들어가면 보이는 메인화면입니다.

상단에 nav 가 위치한 것을 볼 수 있습니다.

main 부분에 지배적으로 보일 컨텐츠가 담긴 영역인 section 이 들어갑니다.

![ig-feed](/assets/img/posts/2022-04-27-semantic-web/4-ig-feed-ppt.png)

section 안에서 존재 자체로 독립적인 의미를 가지는 유저의 피드 는 하나의 article 로 표시됩니다.

많은 피드들이 article 태그로 만들어져 있는 것을 볼 수 있습니다.

![ig-feed-detail](/assets/img/posts/2022-04-27-semantic-web/5-ig-feed-detail-ppt.png)

피드 안에 위치하나 다른 화면에서 다시 독립적으로 사용되는 부분들은 section 으로 다시 정의되어 있습니다.

좋아요 버튼이 담긴 부분, 좋아요 인원표시, 댓글 입력 폼은 메인 페이지 이외에 다른 부분에서 무수히 재사용 되는 부분이기에 별도의 section 으로 처리되어 있는 부분을 볼 수 있습니다. section 안에 article 이 들어갈 수 있고, article 안에 다시 section 이 들어갈 수 있음을 잘 보여줍니다.

## 참고자료

- [HTML 요소 참고서 - MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Element)
- [시맨틱웹 - Wikipedia](https://ko.wikipedia.org/wiki/%EC%8B%9C%EB%A7%A8%ED%8B%B1_%EC%9B%B9)
- [HTML5 - Wikipedia](https://ko.wikipedia.org/wiki/HTML5)
- [시맨틱 태그란?](https://velog.io/@syoung125/%EC%8B%9C%EB%A7%A8%ED%8B%B1-%ED%83%9C%EA%B7%B8-Semantic-Tag-%EC%9E%98-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
- [시맨틱 요소와 검색 엔진](https://poiemaweb.com/html5-semantic-web)
