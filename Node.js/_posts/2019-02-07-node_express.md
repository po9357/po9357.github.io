---
layout: post
title: <p>[Node] 익스프레스(Express) 모듈</p>
description: >
  http 모듈만 사용해서 서버를 구성할 수도 있지만, 이 경우엔 직접 설정해야 하는것들이 많아진다. <br>
  이를 해결하기 위한 **익스프레스(express) 모듈**에 대해 알아본다.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

전 포스트에서 http 모듈을 이용한 서버 다루는법을 알아보았다. <br>
http 모듈만을 사용해 서버를 구성할 경우엔 기타 설정이나 함수 등을 직접 만들어야 하는 불편함이 있다.<br>
**익스프레스(Express) 모듈**을 사용하면 이러한 단점을 보완하고 훨씬 간단한 코드로 서버를 다룰 수 있다.

**Express 모듈**은 외장 모듈로 npm을 이용해 다운 후 사용할 수 있다.<br>
express 모듈을 이용해 생성한 서버객체가 가진 주요 메소드들을 미리 알아보면<br>

| 함수명                               | 설명                               |
|:-------------------------------------| :--------------------------------- |
| set(name, value)                     | 서버 설정을 위한 함수.             |
| get(name)                            | 설정된 서버 속성을 꺼내올 수 있다. |
| use([path], function, [function...]) | 미들웨어 함수 사용                 |
| get([path], function)                | 특정 경로로 요청 정보 처리         |


express 모듈을 이용한 간단한 서버 예제 :

<script src="https://gist.github.com/po9357/4c0fd94db684671905d25130e4825ad7.js"></script>

<br>

## 미들웨어

**익스프레스(Express)**에서 요청이 들어옴에 따라 응답 까지의 중간과정을 함수로 분리해 사용한다.<br>
이렇게 분리한 각각의 것들을 **미들웨어**라고 부른다. 쉽게 서버와 클라이언트를 이어주는 중간작업이라 생각하면 된다.

미들웨어는 **use()**를 이용해 등록을 할 수 있고, **next()**를 이용해 다음 미들웨어로 넘길 수 있다.


### static 미들웨어

**static 미들웨어**를 사용하면 프로젝트내 폴더를 특정 패스로 접근할 수 있다.<br>
``` npm install serve-static --save ```<br>명령어로 설치 후 사용 가능하다.<br>


<script src="https://gist.github.com/po9357/f53b84e20d43d872509644f7ce627bdf.js"></script>

<br>
위 예제처럼 코드를 작성하면 public 폴더 내 파일에 대해서는 <br>
``` http://localhost:3000/public/index.html ```<br>
위와 같이 접근이 가능하고, common 폴더에 대해서는<br>
``` http://localhost:3000/index.html ```<br>
위와 같이 접근이 가능하다.


### body-parser 미들웨어

**body-parser 미들웨어**는 POST 요청에 대해 처리한다.<br>
**GET방식**의 경우 url 주소 뒤에 queryString으로 파라미터들이 넘어오지만, <br>
**POST방식**의 경우엔 요청 본문(body 영역)에 담겨 오기때문에 GET방식과 처리 방법이 다르다.

POST방식은 body-parser 미들웨어를 사용하면 http 모듈을 이용했을때 보다 쉽게 파라미터를 처리할 수 있다.<br>
일단 아래와 같은 파라미터 전달용으로 public 폴더내 login.html 파일을 하나 만든다.


<script src="https://gist.github.com/po9357/d38b6af43266a7bf66b1ee463907bae4.js"></script>


서버 부분을 아래와 같이 작성한다. 


<script src="https://gist.github.com/po9357/cc43057d26e5e4b71ad061ae23a5a4b4.js"></script>

<br>
``` http://localhost:3000/login.html ```<br>
위 주소로 접속한 뒤에 id와 비밀번호를 입력한 후 전송을 하면 <br>
서버에서 파라미터를 받아 처리한걸 볼 수 있다. 

파라미터를 받는 과정에서 **req.body.id || req.query.id**와 같이 작성했다.<br>
**req.body.id**는 body-parser 미들웨어를 이용해 req객체 안 body객체에 저장된 id라는 파라미터를 뜻하고, <br>
**req.query.id**는 GET방식으로 넘어왔을 경우 queryString에 정의된 id라는 파라미터를 뜻한다. <br>
따라서 || 를 이용해 작성해주면, POST방식과 GET방식 모두 처리할 수 있게 된다.


### 라우터

**라우터(Router)**는 클라이언트의 요청이 들어오면, 해당 요청의 경로에 맞는 곳으로 보내주는 역할을 한다.<br>
이러한 역할을 **라우팅(Routing)**이라 부른다. 스프링MVC의 컨트롤러 안의 각각 패스를 지정한 메소드 역할을 한다고 보면된다.