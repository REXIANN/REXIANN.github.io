---
layout: post
title: "빅데이터 프로젝트를 위한 사전 공부 2"
tags: [Self Study, study, bigdata, project]
excerpt_separator: <!--more-->

---

# 2. 기본 추천 시스템 구축

<!--more-->

추천 시스템은 정보 필터링(Information Filtering System) 기술의 일종으로, 특정 사용자가 관심을 가질만한 정보(영화, 음악, 책, 뉴스, 이미지 ,웹 페이지 등)를 추천하는 것이다. 추천시스템에는 대표적으로 컨텐츠 기반 필터링(Content Based Filtering; CBF)과 협업 필터링(Collaborative Filtering; CF)이 있으며 각 방법마다 장단점과 상대적으로 더 적합한 상황들이 있으므로 이를 구분하고 사용하기 위해서는 각 방법론의 특성과 동작방식을 잘 이해해야만 한다.



## 2-1. 컨텐츠 기반 필터링 (Content Based Filtering, CBF)

컨텐츠 기반 필터링은 사용자와 아이템 각각에 대한 이해를 바탕으로 추천하는 방법론이다. 그렇기 때문에 아이템의 특성을 뽑아내는 것이 중요하며 어던 특성을 뽑아야 하는지는 추천하려는 아이템에 따라 달라진다. 협업 필터링에서 일어날 수 있는 cold start problem을 해결할 수 있는 것 또한 컨텐츠 기반 필터링이다. 

사용자의 아이템(상품)에 대한 기록 정보를 바탕으로 특성 벡터를 추출하여 수치화함으로써 각 사용자가 무엇을 좋아할 지를 예측하는 기법으로, 각 아이템들이 가지고 있는 메타 정보들을 특징으로 활용하므로 새로이 추가되는 아이템일지라도 해당 아이템의 메타 정보를 기반으로 하여 특징을 뽑아내고 기존의 아이템목록에서 유사도가 높은 아이템을 추출해 낼 수 있다. 컨텐츠 기반 필터링은 아이템 자체에서 정보를 뽑아내기 때문에 딥러닝이 개입할 여지가 많다. 과거에는 손으로 만든 특징(features) 들을 모아서 사용하기도 했었는데 컴퓨터성능이 좋아지고 데이터가 훨씬 많아진 오늘날에는 손으로 피쳐를 만들기보다는 딥러닝을 많이 사용하고 있다. 

컬럼을 정하고 -> 벡터를 만들고 -> Cosine similarity를 구함 -> 관련 컨텐츠를 찾아냄 

객관적 특성의 경우, 아이템의 메타 데이터를 적절히 가공하여 벡터화 시킬 수 있고, 리뷰 같은 텍스트 데이터의 경우에는 자연어 처리(Natural Language Processing) 기법을 통해 핵심 단어를 찾거나 그 단어들을 임베딩하여 처리하는 기술들이 들어간다. 포스터나 영상미 같은 이미지, 비디오 데이터를 통해 정보를 얻어내려면 이미지 분석(Image Processing)기법이 필요한데 최근에는 딥러닝 모델들이 좋은 성능을 내고 있다. 

처음부터 이런 기법을 다 이해하기는 어렵기 떄문에 선택한 데이터 도메인(텍스트, 이미지, 음성 등) 에 적합한 방법들을 조사하고 전처리 및 간단한 알고리즘부터 테스트해보며 비교해보아야 한다!

[<u>기타 참고자료</u>]

[데이터 도메인 별 기술](https://brunch.co.kr/@kakao-it/72)

[대표적인 기법들](https://www.samsungsemiconstory.com/2265)

[해리포터로 알아보는 TF-IDF기법의 의미](https://blog.naver.com/myincizor/221823805086)

[TF-IDF기법의 장단점 및 시각화](https://donghwa-kim.github.io/TFIDF.html)

[Word2Vec을 사용한 텍스트 리뷰 임베딩](https://medium.com/qandastudy/python%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%BD%B4%EB%8B%A4-%EB%A6%AC%EB%B7%B0-%EB%B6%84%EC%84%9D-73b3f26e967c)

[doc2vec을 이용한 네이버 영화 리뷰 감정 분석](http://hero4earth.com/blog/projects/2018/01/21/naver_movie_review/)





## 2-2. 협업 필터링 (Collaborative Filtering, CF)

컨텐츠 기반 필터링이 아이템의 특성을 뽑아내어 그 정보를 추천에 사용한다면, 협업 필터링은 이미 사용자들의 리뷰 또는 시청 여부, 구매 여부를 통해 얻어진 데이터를 이용해 추천하는 방식이다. 협업 필터링은 사용자의 아이템(또는 상품)에 대한 기록 정보를 바탕으로 특성벡터를 머신러닝 방식으로 자동적으로 수치화하여 각 사용자가 무엇을 좋아할 지를 예측한다. 다른 사람이 남긴 기록(좋아요, 매긴 평점, 남긴 리뷰, 기타)을 기반으로 하는 추천시스템으로, 협업 필터링을 활용한 추천에서는 깊은(Deep)모델보다는 얕은 모델을이 아직까지는 우위를 점하고 있다. 협업 필터링의 대표적인 장점은 **상품 그 자체를 이해하지 않고도 영화와 같은 복잡한 아이템을 추천할 수 있다는 것** 이다. 이 때 포인트는 '많은 사용자들로부터 얻은 정보' 라고 볼 수 있다.

세부적으로는 Memory-Based 방식과 Model-Based 방식이 있으며 Memory-Based방식은 또 다시 user-based와 item-based방식으로 나뉜다. 

### 2-1-1. Memory-Based 방식

####  user-based collaborative filtering

말 그래도 비슷한 유저를 골라내는 것으로, 어떤 유저와 비슷한 성향 또는 취향을 가진 유저들을 찾아내어 그 유저들의 정보를 기반으로 해당 유저에게 맞춤 정보를 제공한다. 보통 구매 기록 등을 통해 같은 제품을 구매한 기록이 있는 사용자들을 찾고 이를 바탕으로 아이템을 추천하는 방식이다. 

####  item-based collaborative filtering

하나의 아이템을 선택 또는 평가한 유저들을 정리하고 그 아이템에 대한 평가가 유사한 유저들이 선택한 다른 아이템들을 해당 유저에게 추천하는 방법. 아이템의 특성을 어떻게 정의하느냐에 따라 같은 부류로 묶이는 아이템들이 달라지므로 여러 특성을 적용해 보고 효과가 좋은 특성을 찾아내는 것이 중요하다.

### 2-1-2. Model-Based 방식

User-Item행렬이 임의의 잠재 요소들을 통해 결정된다는 가정 하에 User-Item 행렬을 User-Latent Feature 행렬과 Item-Latent Feature 행렬로 분리하고 분리된 각 행렬의 값을 최적화를 통해 알아내는 머신러닝적 접근 방법이다. 정확히 어떤 요소가 구매나 시청, 리뷰 점수로 이어졌는지는 파악하기 힘들지만 기록이 어느 정도 쌓였다는 가정 하에 단순한 컨텐츠 기반 필터링보다 성능을 크게 개선할 수 있다.

### 2-1-3. 행렬 분해(Matrix factorization)

행렬 분해랑 말 그래도 매트릭스를 분해하는 방법이다. 유저가 어떤 컨텐츠를 소비했는지를 유저-컨텐츠 매트릭스로 표현한 후 이 매트릭스를 랭크가 작은 두개의 매트릭스로 나누고, 이렇게 두 개로 나눈 매트릭스의 곱이 원래의 매트릭스와 비슷하게 나오도록 학습하는 방법이다. 원래의 유저-컨텐츠의 랭크보다 적은 숫자로 인풋을 학습하는 것이기 때문에, 압축하고 복원하는 과정에서 원래 유저가 소비하지 않았던 컨텐츠에 대한 숫자들이 나오게 되는데 이 숫자들을 유저에 대한 해당 컨텐츠의 **계산된 선호도** 라고 볼 수 있다. 

### 2-1-4. 오토인코더(Autoencoder)

모델에서 더 다양한 정보를 얻고 싶을 경우 위의 행렬분해보다 더 유연한 오토인코더를 사용할 수 있다. 오토인코더는 행렬 분해와 비슷하게 입력 데이터를 원래의 차원(dimension)보다 적은 데이터로 줄인 후에, 변환한 데이터를 기반으로 원래의 데이터를 복원하는 구조로 되어 있다. 

오토인코더가 학습하고 데이터를 생성하는 과정은 한 문장의 설명을 듣고 몽타주를 그리는 과정과 유사하다. 몽타주는 초상화와 다르게 제한된 정보를 이용해 최대한 원본에 가깝게 그려야 하기 때문에 제공되는 정보는 높은 가치가 있어야 한다. 또한 설명을 듣고 그림을 그리는 사람의 실력도 좋아야 한다. 오토인코더는 몽타주를 그리는 과정에서 한 문장을 설명하는 사람과 이를 듣고 몽타주를 그리는 사람이 서로 합을 맞춰가는 과정과 유사핟. 몽타주 대상을 설명하는 문장에 최대한 많은 정보를 담고, 결과물인 몽타주가 최대한 원본에 가깝도록 학습해 나가는 모델이다. 설명을 할 때 강제로 정보를 축약해서 표현하고 이를 복원하는 과정에서 **원본 값이 그대로 나오기보다 원본에서 가장 중요한 핵심 값들이 나오는 방식**이라고 할 수 있다. 

협업 필터링의 경우 기본적으로 과거의 선택을 기반으로 미래의 선택을 추천 하는 것으로 새로운 아이템이 생겼을 경우 이 아이템을 어떤 유저에게 추천해야할 지 결정하기 힘들다. 이것을 **cold start problem** 이라고 부른다. 

[<u>기타 참고자료</u>]

[Collaborative Filtering의 전반적 흐름](https://realpython.com/build-recommendation-engine-collaborative-filtering/)

[Memory Based CF의 기본 개념](https://scvgoe.github.io/2017-02-01-%ED%98%91%EC%97%85-%ED%95%84%ED%84%B0%EB%A7%81-%EC%B6%94%EC%B2%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-(Collaborative-Filtering-Recommendation-System)/)

[User Based CF 구현](https://medium.com/sfu-cspmp/recommendation-systems-user-based-collaborative-filtering-using-n-nearest-neighbors-bf7361dc24e0)

[Item Based CF 구현](https://lsjsj92.tistory.com/568?category=853217)



## 2-3. 추천 시스템 특화 라이브러리

추천 시스템에 특화된 라이브러리 또는 프레임워크로는 LightFm, Surprise, PySpark가 있다. 해당 라이브러리들은 복잡한 추천 알고리즘을 상당히 최적화된 상태로 만들어서 제공해주기 때문에 빠르게 추천 시스템을 구추갛는데 도움을 준다. 

`LightFM`은 기본 추천 시스템에 필요한 알고리즘뿐만 아니라 하이브리드 추천 시스템 구축, 아직 데이터가 충분하지 않은 cold-start상황에서의 추천 등 다양한 상황을 지원하는 라이브러리이다. 

`Surprise`는 협업 필터링 추천 시스템을 위한 알고리즘을 제공하는 파이썬 라이브러리이다. scikit-learn의 핵심 개발자 중 한명인 nicolas Hug가 주축이 되어 관리되고 있다.

`PySpark` 는 아파치 스파크 프레임워크의 파이썬 API이다. 스파크는 분산 클러스터 컴퓨팅 용도로 만들어진 오픈 소스 프로젝트이며 다양한 머신러닝 알고리즘을 포함하고 있다. 처름부터 대용량 데이터 처리를 위해 만들어졌기 때문에 위의 두 라이브러리보다 추천에 특화된 알고리즘 종류느 적지만 대용량 처리와 속도 면에서 이점이 있다.

[LightFM 공식 문서](https://making.lyst.com/lightfm/docs/index.html)

[LightFM 예시 코드](https://towardsdatascience.com/recommendation-system-in-python-lightfm-61c85010ce17)

[Surprise 공식 문서](https://surprise.readthedocs.io/en/stable/)

[Surprise Tutorial](https://blog.cambridgespark.com/tutorial-practical-introduction-to-recommender-systems-dbe22848392b?gi=c1197c31f7b4)

[PySpark 공식 문서](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#module-pyspark.mllib.recommendation)

[PySpark ALS알고리즘 사용 튜토리얼](https://www.youtube.com/watch?v=FgGjc5oabrA)

[PySpark를 번개장터 추천시스템에 사용한 후기](https://www.theteams.kr/teams/7937/post/70673)



## 2-4. 유사도 측정 지표(Similarity Metric)

컨텐츠 기반 필터링 기법을 사용하는지, 협업 필터링 기법을 사용하는지에 관계없이 공통적으로 유사한 유저, 유사한 아이템, 유사한 잠재 벡터를 찾기 위해서는 유사도를 수치화할 지표가 필요하다. 유사도 지표는 다양하며 어떤 방법을 사용하는지에 따라 같은 벡터간의 결과값도 서로 다르게 나오며 실제로도 여러 지표들의 결과를 적절히 조합하여 사용한다. 

유사도를 측정하는 대표적인 방법에는 유클리드 거리(Euclidean distance), 코사인 유사도(Cosine similarity), 피어슨 상관 계수(Pearson correlation coefficient)등이 있다.

[유사도의 기본 개념](https://www.fun-coding.org/recommend_basic3.html)

[유클리디안 거리를 포함한 다양한 유사도 측정 지표들](https://forensics.tistory.com/49)

[코사인 유사도를 사용한 추천 시스템 구축](https://wikidocs.net/24603)

[피어슨 상관계수를 포함한 다양한 유사도 지표](https://towardsdatascience.com/collaborative-filtering-based-recommendation-systems-exemplified-ecbffe1c20b1)

[유사도 지표 비교에 따른 같은 벡터의 다른 계산](https://developers.google.com/machine-learning/recommendation/overview/candidate-generation)





