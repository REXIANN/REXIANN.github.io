---
layout: post
title: "App memory, Wired Memory, Compressed"
tags: [macOS, memory, RAM]
excerpt_separator: <!--more-->
sitemap :
  changefreq : daily
  priority : 1.0



---

매킨토시를 사용하다 보면 이런저러한 이유로 인해 내 컴퓨터의 CPU 사용량(또는 특정 프로세스의 점유율), 메모리 사용량을 살펴봐야 할 때가 있다. 

<!--more--> 

어플리케이션 중 기본적으로 설치되어 있는 Activity Monitor를 켜서 Memory 탭을 열면 된다. 나는 언어를 영어로 설정했는데, 한국어의 경우 활성 상태 보기 앱을 켜면 된다.

![Screen Shot 2021-11-28 at 8.51.01 PM.png](/assets/img/posts/2021-11-27-app-wired-compressed-memory-macos/Screen_Shot_2021-11-28_at_8.51.01_PM.png)

내가 사용하는 맥북은 일부러 돈을 더 주고(~~내돈...~~) 램을 32기가로 늘린 모델이다.

보면 다음과 같이 Physical Memory, Memory Used, Cached Files, Swap Used가 있다. 뭐 Physical Memory는 물리적으로 꽂혀있는 메모리 용량일 것이고.. 캐시된 파일은 캐시된 파일일 것이고... 스왑은 메모리 스왑에 사용된 부분일 것이다.

근데 맨 왼쪽에 메모리압력은 뭘까? 하는 생각이 들었다. 뭔가 외부에서의 압박이 들어오는 상황을 압력이라고 부르는 것 같은데 정확하게 무엇이 어떤 압력을 주고 있는걸까?

그리고 지금 사용되고 있는 메모리가 17.82GB인데 각각 App Memory, Wired Memory, Compressed 라고 되어 있다. 이건 또 뭘까?

몰라도 상관없지만 갑자기 궁금해져서 찾아봤다.

## Memory Pressure (메모리 압력)

메모리를 프로세스의 요구에 맞춰 얼마나 효율적으로 사용하는지 그래픽으로 표시해준다. 메모리 압력은 프리 메모리, 스왑률, 와이어드 메모리 및 캐시 파일 메모리로 결정된다고 한다.

높은 메모리 압력은 시스템이 한계에 도달하고 있음을 의미하며, 성능이 저하(degrade)될 수 있다.

## App Memory (앱 메모리)

말 그대로 앱에서 사용 되고 있는 메모리의 크기이다. 

## Wired Memory (와이어드 메모리)

시스템 작동에 필요한 메모리이다. 

이 메모리는 캐시될 수 없으며 주기억장치에 유지되어 있어야 하기 때문에 다른 앱에서 사용할 수 없다.

일반적으로 이 메모리는 OS X의 핵심 기능에서 사용되며 커널에 속한다. 

어떻게 보면 다른 메모리를 감독하는 메타 메모리로 볼 수도 있다고 한다. 

## Compressed (압축)

압축된 메모리의 크기를 의미한다. 

컴퓨터의 메모리 사용량이 최대에 도달한 경우, 비활성화 되어 있는 앱의 메모리를 압축하여 활성화된 앱이 사용할 수 있는 추가적인 메모리 공간을 확보한다. 이 때 압축이 된 데이터가 차지하고 있는 용량을 의미한다.

## 결론과 꿀팁

더 자세한 건 애플의 공식지원센터에 등재되어 있는 [문서](https://support.apple.com/ko-kr/guide/activity-monitor/welcome/mac#memory)를 보자. 목차 버튼을 누르면 활성 상태 보기 앱의 각 항목에 대한 설명을 자세하게 읽어 볼 수 있다.

그런데 찾아보다 쇼킹한 부분을 발견했다....

활성상태보기 앱의 각 항목에 마우스를 올려두고 잠시 가만히 있으면 설명을 보여준다.... 

나의 구글링이 무색해지는 순간이었다...

![Screen Shot 2021-11-28 at 9.11.27 PM.png](/assets/img/posts/2021-11-27-app-wired-compressed-memory-macos/Screen_Shot_2021-11-28_at_9.11.27_PM.png)