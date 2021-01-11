---
layout: post
title: "빅데이터 프로젝트를 위한 사전 공부 1"
tags: [Self Study, study, bigdata, project]
excerpt_separator: <!--more-->


---

# 1. 데이터 분석 및 시각화

<!--more-->

## 1-1. 개요

현재 수많은 기어베서 매출을 극대화하기 위해 추천 시스템을 도입하고 있다. 대표적인 예로 Netflix에서 시청되는 영화의 80%, Youtube에서 재생되는 동영상의 60%, Amazon 전체 매출의 35%가 추천 시스템에 의해서 발생한다. 경제적 산업적 파급효과가 막대한 추천 시스템의 중요성을 일찍이 인지한 많은 기업들의 대량의 사용자 로그 데이터를 활용한 추천 시스템 연구를 10년 이상 지속해오고 있다.

서비스 사용자의 관점에서 추천시스템은 수천만개의 컨텐츠 및 제품에 대한 탐색 시간을 줄여주고 자신의 과거 기록이나 작성한 정보, 다른 유저들의 행동패턴 등을 통해 적합한 후보들을 골라준다는 이점이 있다. 이렇게 적합한 후보를 제시함으로써 기업은 사용자의 편의성을 극대화 할 수 있고, 이는 더 많은 서비스 구독이나 판매로 이어져 결과적으로는 서비스 제공자와 이용자 모두 이득을 보는 시스템이다. 

## 1-2. EDA(Exploratory Data Analysis)

데이터 분석 및 시각화를 위해 다음의 과정을 거친다.

* Pandas 라이브러리를 사용하여 주어진 데이터를 정제하고 통계적 분석을 수행
* Matplotlib, Folium 라이브러리를 사용하여 데이터를 시각화

이 과정을 탐색적 데이터 분석(EDA) 이라고 하며 어떤 목표를 가지고 데이터분석을 진행하던지에 상관없이 1. 주어진 데이터에 어떤 값들이 있는지 2.값들은 수치로 표현되어 있는지 혹은 카테고리 값으로 표현되어 있는지 3. 특성들 사이에는 어떤 관계가 있는지 데이터를 이해하는 단계이다.

주어진 데이터의 특성들을 조합해 새로운 특성값을 만들어 내서 사용해야 하므로 데이터와 알고리즘에 대한 깊은 이해가 필요하다.

## 1-3. Python & Pandas DataFrame

### 1-3-1. Python

파이썬은 매우 간결하면서도 강력한 기능을 가진 프로그래밍 언어이다. Pandas, Numpy, Scipy, Scikit-learn등의 라이브러리를 사용하여 간편하게 수치 연산과 데이터 분석을 할 수 있으며, 다양한 빅데이터 알고리즘 또한 미리 구현되어 있어 데이터 과학에 적합한 언어라 할 수 있다. 구글의 Tensorflow나 Facebook의 PyTorch를 활용하여 딥러닝 분야에서도 파이썬이 널리 사용되고 있으므로 데이터 과학자나 머신 러닝 엔지니어를 지망하는 개발자라면 필수적으로 알아야 하는 언어이다.

### 1-3-2. Numpy

Numpy는 다차원 배열의 행렬 연산을 빠르게 수행하기 위한 데이터 구조를 제공하는 파이썬 라이브러리이다. C기반으로 작성되어 있어 매우 빠르며 데이터 구조 외에도 선형대수학, 푸리에 변환 등 유용한 함수를 같이 제공한다. 

### 1-3-3. Scipy

Scipy는 계산과학 분야에 사용되는 다양한 수학 알고리즘을 모아놓은 파이썬 라이브러리이다. numpy를 기반으로 작성되어 있으며, Numpy가 기본적인 데이터 구조와 연산을 제공한다면, scipy는 numpy의 데이터 구조를 활용하여 다양하고 복잡한 알고리즘ㅇ르 구현하여 제공한다.

### 1-3-4. Scikit-learn

Scikit-learn 은 데이터 마이닝 및 분석에 필요한 효과적인 기능을 제공해주는 파이썬 라이브러리이다. numpy 및 scipy로 만들어졌으며 데이터 전처리를 비롯하여 분류, 클러스터링, 차원 축소, 회귀 등의 다양한 머신 러닝 기법을 파이썬 함수로 손쉽게 사용할 수 있게 한다.

### 1-3-5. Pandas

Pandas는 데이터 정제 및 분석을 위해 사용하는 파이썬 라이브러리이다. DataFrame 이라는 관계형 데이터베이스의 테이블을 닮은 2차원 데이터 구조를 주로 사용하며, 이 데이터 구조에 대하여 여러가지 처리를 할 수 있는 기능을 갖추고 있다. Pandas의 주요기능은 C로 구현되어 있기 때문에 대규모 데이터를 다룰 떄의 성능이 좋으며, Numpy나 Matplotlib과 같은 타 파이썬 라이브러리들과의 호환성도 매우 높아 파이썬을 이용한 데이터 분석의 기초가 되는 라이브러리이다.

사용하기 위해서는 설치를 하고 import가 필요하다

```python
# 터미널에서 pip install pandas
import pandas as pd
```

#### Series

Series는 가장 간단한 1차원 자료구조로 배열/ 리스트와 같은 일련의 시퀀스 데이터를 받아들인다. 별도의 인덱스 레이블을 지정하지 않으면 자동으로 0부터 시작하는 디폴트 정수 인덱스를 사용한다.

```python
import pandas as pd
data = [1, 2, 3, 4, 5]
s = pd.Series(data)

```

#### DataFrame

DataFrame은 2차원 자료구조로 행과 열이 있는 테이블 데이터를 받아들인다. 아래 예제는 그 방법 중 한가지로 열(column)을 dict의 key로, 행(row)을 dict의 value로 한 딕셔너리형 데이터를 pd.DataFrame()을 사용하여 pandas의 DataFrame자료구조로 변환한 예이다.

```python
import pandas as pd
data = {
  'year': [2016, 2017, 2018],
  'GDP rate': [2.8, 3.1, 4.0],
  'GDP': ['1,637M', '1.73M', '1.83M']
}
df = pd.DataFrame(data)
```

#### Panel

Panel은 3차원 자료구조로 Axis 0, Axis 1, Axis 2 (각각 items, major_axis, minor_axis)의 세개의 축을 가지고 있다. Axios 0 은 그 한 요소가 2차원의 DataFrame에 해당되며, Axis 1 은 DataFrame의 행(row)에 해당되고, Axis 2 는 DataFrame의 열(column)에 해당된다. 

아래 예제는 numpy를 사용하여 3차원 난수를 발생시킨 후, 이를 pandas.Panel()에 적용한 예이다. 출력 결과를 보면 2 x 3 x 4 `(items * major_axis * minor_axis)` 크기의 Panel객체가 생성되었음을 알 수 있다. `p[0]`르ㄹ 출력해보면 Axis 0 의 첫번쨰 요소인 DataFrame이 출력된다.

```python
import pandas as pd
import numpy as np

data = np.random.rand(2, 3, 4)
p = pd.Panel(data)
print(p) # 3차원 객체 Panel
print(p[0]) # 객체의 items
```

#### 데이터 액세스

Pandas에서 Series, DataFrame, Panel 등의 자료구조를 만든 후, 다양한 방법을 통해 데이터에 접근할 수 있다. 가장 간단한 방법으로 pandas 자료구조에 대해 인덱싱 혹은 속성(Attribute)을 사용하는 것인데, 예를 들어 위에서 생성한 DataFrame인 df에 대해 year행을 가져오기 위해서는 

```python
df['year'] # 해당 column에 있는 값들을 가져옴
df.year # 위와 동일한 표현
```

또한 불린 인덱싱(Boolean indexing)을 사용하여 특정 조건의 데이터를 필터링 할 수도 있다. 예를 들어 2016년 이후의 데이터만 필터링 하기 위해서는 

```python
df[ df['year'] > 2016] # Boolean indexing
```

또한 해당 컬럼의 총합 또는 평균도 간단하게 구할 수 있다.

```python
gdp_sum = df['GDP rate'].sum()
gdp_avg = df['GDP rate'].mean()
print(sum, avg) # 8.9, 2.97
```

데이터의 양이 많은 경우 df.head()함수를 사용하면 처음 5개를 표시해주며, df.tail()함수를 사용하면 마찬가지로 마지막 5개 row를 표시해 준다.

### 1-3-6. Matplotlib

Matplotlib은 데이터 시각화를 위해 사용되는 파이썬 라이브러리로, 다양한 형태의 그래프를 손쉽게 그릴 수 있으며, 통계 기능과 색 테마를 지원하는 Seaborn 라이브러리를 함께 활용하면 더욱 간편하게 그래프를 그릴 수 있다. 

아래 예제는 csv파일을 pandas를 통해 읽어 들인 후, amtplotlib을 통해 bar차트를 그리는 예제이다.

```python
%matplotlib inline
import pandas as pd
from amtplotlib import pyplot as plt

df = pd.read_csv('test.csv')
plt.bar(df.ID, df['column name'])
plt.show()
```



### 1-3-7. Folium

Folium은 지도 데이터를 사용하여 위치 정보를 시각화하는 데에 사용되는 파이썬 라이브러리이다.


