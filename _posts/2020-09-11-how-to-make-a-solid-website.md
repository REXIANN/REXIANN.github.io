---
layout: post
title: "튼튼한 웹사이트를 만들기 위하여(feat. Django)"
tags: [Python, Django, HTTP methods, Web]
excerpt_separator: <!--more-->


---

### Question!

"Django에서 CRUD로직을 작성할 때,  Create, Update 의 경우 로직 상 구현은 GET 부터 하는데 ,
왜 조건 분기에서는 `request.method == 'POST'` 를 먼저 작성할까?"

<!--more-->

### GET 먼저 분기 - 1

```python
if request.method == 'GET':
    form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/index.html', context)
else:
    form = AuthenticationForm(request.POST)
    if form.is_valid():
        form.save()
        return redirect('accounts:index')
    else:
        context = {
            'form': form
        }
        return render(request, 'accounts/index.html', context)
```

- 불필요한 코드의 반복..



### GET 먼저 분기 - 2

```python
# 그럼 위 1번 코드가 반복이라면 이렇게 줄이면 되잖아?
if request.method == 'GET':
    form = AuthenticationForm()
else:
    # 하지만 이렇게 하면 여기서 POST가 아니라 PUT, PATCH, DELETE 등 다른 메서드로 요청이 와도 여기가 실행되는거 문제가 생기는데?
    form = AuthenticationForm(request.POST)
    if form.is_valid():
        form.save()
        return redirect('accounts:index')
context = {
    'form': form,
}
return render(request, 'accounts/index.html', context)
```

- 잠깐 그건 우리가 원래 작성하던 방식대로해도 같은 이야기 아니야?

  ```python
  if request.method == 'POST':
      form = AuthenticationForm(request.POST)
      if form.is_valid():
          form.save()
          return redirect('accounts:index')
  else:
      # 이렇게 해도 여기서 GET이 아닌 다른 메서드로 요청이 오면 여기가 동작 되어버리잖아.
      form = AuthenticationForm()
  context = {
      'form': form,
  }
  return render(request, 'accounts/index.html', context)
  ```

  - 네 둘 다 맞는 이야기 입니다.



다만 if 문을 지나 else 문이 실행 될 때의 2가지 상황을 잘 생각해봅시다. (`else` 일 경우에 집중하세요.)

첫번째 코드를 먼저 살펴 봅시다.

```python
if request.method == 'GET':
    form = AuthenticationForm()
else:
    form = AuthenticationForm(request.POST)
    if form.is_valid():
        form.save()
        return redirect('accounts:index')
```

- 여기서 else 구문은 http method가 GET이 아닐 때 즉, POST, PUT, DELETE 등 다른 메서드 일 때 수행됩니다.



두번째 코드(우리가 배운)도 살펴 봅시다.

```python
if request.method == 'POST':
    form = AuthenticationForm(request.POST)
    if form.is_valid():
        form.save()
        return redirect('accounts:index')
else:
    form = AuthenticationForm()
```

- 여기서 else 구문은 http method가 POST가 아닐 때 즉, POST, PUT, DELETE 등 다른 메서드 일 때 수행됩니다.



여기서 중요한 점은 else 구문이 수행될 때의 코드입니다.

첫번째 코드에서의 else는 `save()`가 있는 DB에 무언가 조작을 하는 코드입니다.

그런데 두번째 코드에서의 else는 단순히 form 인스턴스를 생성하는 코드입니다.

의미론적으로 생각해봅시다. 

**사용자가 내 예상과 다른 method로 요청했을 때 단순히 form 인스턴스를 생성하는 코드를 수행하게 하실건가요 아니면 DB에 무언가 조작을 수행하는 코드를 수행하게 하실 건가요?**

당연히 단순히 DB에 조작을 가하지않는 코드를 수행하는 것이 맞지 않을까요?



---



그렇다면 의미론 적인 이유 말고도 실제로 어떻게 동작하는지 보겠습니다.

현재 테스트하는 view 는 다음과 같습니다.

```python
def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```



[POSTMAN](https://www.postman.com/) 이라는 프로그램을 통해 회원가입 페이지에 `PUT` 메서드로 요청을 보내보겠습니다.

<img width="1020" alt="Screen Shot 2020-04-14 at 5 51 40 PM" src="https://user-images.githubusercontent.com/18046097/79206016-af92d680-7e79-11ea-9fe8-0916077faded.png">

- 403 Forbidden ??

- csrf 검증을 통과하지 못했다고 합니다.

- django는 GET method가 아니면 요청에서 csrf 검증을 시도합니다. 
이 검증에 관한 코드는 `settings.py`에 작성되어 있습니다.
  
  ```python
  # settings.py
  
  MIDDLEWARE = [
      ...,
      'django.middleware.csrf.CsrfViewMiddleware',
      ...,
  ]
  ```
  
- 저 미들웨어의 해당 코드가 해주는 것은 CSRF에 대한 검증입니다.
  
  - 즉, PUT 메서드 요청은 코드 상으로 else 구문이 실행되기는 하겠지만, 애초에 csrf 검증을 통과하지 못하기 때문에 signup 함수는 동작조차 하지 못하고 403 에러가 발생합니다.



그렇다면 저 미들웨어를 주석처리하고 다시 요청을 보내면 어떻게 될까요?

<img width="1057" alt="Screen Shot 2020-04-14 at 5 25 53 PM" src="https://user-images.githubusercontent.com/18046097/79206501-6727e880-7e7a-11ea-8b87-3290d35cf034.png">

- csrf 검증이 동작하지 않게 되고 코드상으로 else 구문이 실행되기 때문에 우리가 GET으로 요청을 보낸 것처럼 회원가입 페이지가 잘 렌더링 됩니다.



그런데 절대로 그럴일은 없겠지만 우연히 csrf_token을 맞춰서 통과를 하게 된다면? 

마찬가지로 회원가입 작성 페이지가 렌더링 될 것입니다.

그래서 이것조차 방지한다고 한다면 이렇게 완성할 수 있습니다.

```python
from django.views.decorators.http import require_http_methods


@require_http_methods(['GET', 'POST'])
def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('accounts:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

- 이렇게 하면 혹여나 csrf_token을 맞춰서 뚫었다고해도 signup 함수는 실행되지 않고 [405 Method Not Allowed](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/405) 를 응답합니다.



### 결국 django가 현재 구조를 선택한 이유는

1. 의미론적으로 POST가 아니면(GET, PUT, DELETE) DB 조작과 관련되지 않은 코드가 실행되도록.
2. 실제 동작에서도 csrf 검증을 통과하지 못하기 때문에 크게 상관은 없음.
3. 만에 하나 검증을 통과하더라도 완벽하게 막아 줄 수 있도록 데코레이터까지 사용.



---



### 참고문헌

https://docs.djangoproject.com/en/2.1/ref/csrf/#module-django.middleware.csrf

https://docs.djangoproject.com/en/2.1/topics/http/decorators/





