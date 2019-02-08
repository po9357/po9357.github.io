---
layout: post
title: <p>[Node] url모듈과 querystring모듈</p>
description: >
  노드의 기본적인 기능 중 url 모듈과 querystring 모듈에 대해 알아본다.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

<h2>url 모듈</h2>

**URL(Uniform Resource Locator)**이란 이름에 써있듯이 네트워크상에서 자원이 어딨는지 알려주기위한 규약이다.<br>
구글 혹은 네이버 등 검색엔진에서 아무 단어나 검색한 후 주소창의 url을 살펴보자.<br>
네이버에 url을 검색하면 이러한 주소가 완성된다.<br>
```https://search.naver.com/search.naver?sm=top_hty&fbm=1&ie=utf8&query=url```<br>
노드에서는 이러한 url을 **URL 객체**화 할 수 있다. 객체화를 통해 원하는 정보를 다루기가 더욱 편리해진다.<br>

**URL 모듈**을 사용해 위의 url을 객체화 한 후 사용해보자.

<br><script src="https://gist.github.com/po9357/f23580bccb99bf09964d592f045ce0d7.js"></script>



## querystring 모듈

위의 url을 보면 ?를 구분자로 뒤에는 sm=top, query=url식으로 표현된걸 알 수 있다.<br>
?뒤에 오는 (key=value)형식들을 **querystring**이라고 한다.<br>
**url 객체**의 속성을 보면 query라는 속성 정보가 있고 이 속성에는 서버로 들어오는 요청 파라미터 정보가 담겨있다.<br>
쉽게말해 클라이언트의 요청 정보가 querystring형식으로 서버에 전송된다 할 수 있다.

**querystring 모듈**을 사용하면 이러한 파라미터들을 쉽게 분리할 수 있다.<br>
위 예제에서 사용한것과 같이 querystring모듈의 **parse()**함수를 이용해 url 객체의 query속성을 빼올 수 있다.<br>
그 후 예제의 마지막 부분과 같이 **query.key값**을 사용해 파라미터 정보를 알 수 있다.

<script src="https://gist.github.com/po9357/c780afa2f36f1ee5fbe0a1cca9778a6e.js"></script>