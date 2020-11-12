# 스크립트 로드에 관해

JS 를 다룰 때, **꼭 알아야 하고**, 알고 나서도 *맨날 헷갈리는*

적절한 **스크립트 위치** 와

**로드 시점** 에 대해 정리해봅니다

------------------

지난 <code>meeansub</code> 님의 [브라우저 동작 원리](https://github.com/Team-ITDA/studyhub/tree/main/web/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC)

글을 읽으며 우리가 보고 있는 웹 브라우저가 어떻게 진행되는 지 알 수 있었습니다

요악해보자면

우리가 작성한 <code>HTML</code> 코드를 브라우저가
1. 해석하면서
2. <code>DOM</code> 트리를 구성하고
3. 그리는 과정을 거칩니다

*여기서 DOM 트리란 Document Object Model, 즉 태그들을 하나의 객체로 보고 트리를 구성한 것입니다. 브라우저는 이렇게 구성된 트리를 하나씩 위에서부터 그려나가죠*

이러한 과정에서 스크립트 구문을 만나면 브라우저가 어떻게 진행할까요?

```html
<html>
<head>
  <script>console.log("영곤짱")</script>
</head>
<body>
  진민짱
</body>
</html>
```

브라우저는 <code>HTML</code> 코드를 위에서부터 아래로 해석하기 때문에

위와 같은 코드를 해석하게 되면 <code>body</code> 태그를 해석하기도 전해

**스크립트를 다운받고 실행하는** 시간이 추가됩니다

![category](category.svg)
![script](script.svg)

즉, 스크립트를 실행하느라고 화면에 <code>HTML</code> 를 그리는 시간이 더 뒤처지게 되는 것입니다

제가 JS를 처음 만질때는

```html
<html>
<head>
  <script>
    const container = document.getELementById("container");
    container.innerText = "영곤짱";
  </script>
</head>
<body>
  <div id="container">
    진민짱
  </div>
</body>
</html>
```

위 코드가 왜 에러가 뜨는지 몰랐었습니다.

지금은 브라우저가 <code>DOM</code> 트리를 구성하기 전에 실행되었으니

브라우저 입장에서 보았을땐 <code>container</code> 아이디 값을 가진 요소가 없다는 것을 알 수 있겠죠??

따라서 가장 안전하고 빠르게 실행되기 위해서는 스크립트를 아래

즉 <code>body</code> 태그 끝에 배치하는 것이 좋습니다


```html
<html>
<head>

</head>
<body>
  <div id="container">
    진민짱
  </div>

  <script>
    const container = document.getELementById("container");
    container.innerText = "영곤짱";
  </script>
</body>
</html>
```

-----------------

스크립트가 적절한 시점에 실행되도록 하는 두 함수가 있습니다


### <code>onload</code> vs <code>DOMContentsloaded</code>


비슷한 역할을 하지만 미묘하게 다른 두 메소드에 대해 정리해봅니다

브라우저는 <code>DOM</code> 트리를 구성하고

외부 자원, 예를 들면 <code>css</code> 나 이미지 파일 같은 것들을 로드해와서 붙이는 과정을 거칩니다

- <code>DOMContentsloaded</code> 는 <code>DOM</code> 트리를 구성하고 실행되서 **외부 자원을 가져오기 전에 실행** 됩니다

- <code>onload</code> 는 <code>DOM</code> 트리를 구성하고 **외부 자원을 가져온 후에 실행** 됩니다

*맨날 헷갈리는 부분이니 차이점을 정확히 알고 가세요*

----

따로 작성한 스크립트 파일을 <code>HTML</code> 파일에 불러올 때,

관련 속성으로

<code>defer</code>와 <code>async</code>가 있습니다



### <code>defer</code> vs <code>async</code>

- ### <code>defer</code>
  <code>defer</code> 속성을 선언하게 되면 브라우저가 스크립트를 만났을 때, 스크립트를 다운받고 실행은 **<code>HTML</code> 코드 해석 후에 실행** 하도록 뒤로 미룹니다

  ![category](category.svg)
  ![script](defer.svg)



- ### <code>async</code>
  <code>async</code> 속성을 선언하게 되면 브라우저가 스크립트를 만났을 때, **<code>HTML</code> 코드 해석과 동시에** 백그라운드에서 스크립트를 실행합니다

  ![category](category.svg)
  ![script](async.svg)


-----

끝으로

<code>defer</code> 와 <code>async</code> 의 경우

지원하지 않은 구형 브라우저가 있음을 고려하시고

이러한 부분을 신경쓰신다면

스크립트를 <code>body</code> 태그 끝으로 내리는 것이 마음 편하다는 점 알아두셨으면 합니다







> 원문
> [브라우저의 역할과 스크립트의 로드 시점](https://webclub.tistory.com/630)
