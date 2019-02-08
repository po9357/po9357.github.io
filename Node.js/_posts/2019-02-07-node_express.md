---
layout: post
title: <p>[Node] 익스프레스(Express) 모듈</p>
description: >
  http 모듈만 사용해서 서버를 구성할 수도 있지만, 이 경우엔 직접 설정해야 하는것들이 많아진다. <br>
  이를 해결하기 위한 **익스프레스(express) 모듈**에 대해 알아본다.
image: /assets/img/programming.jpg
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
|:-------------------------------------| :--------------------------------: |
| set(name, value)                     | 서버 설정을 위한 함수.             |
| get(name)                            | 설정된 서버 속성을 꺼내올 수 있다. |
| use([path], function, [function...]) | 미들웨어 함수 사용                 |
| get([path], function)                | 특정 경로로 요청 정보 처리         |

