---
layout: post
title: <p>[Node] Event</p>
description: >
  노드는 이벤트를 기반으로 비동기 방식 처리를 주로 한다. <br>따라서 이벤트를 이해하면 node를 더욱 잘 활용할 수 있다.
image: /assets/img/programming.jpg
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

<h2>Event</h2>

노드에서 **Event**란 쉽게말해 한쪽에서 다른 쪽으로 어떤 일이 발생했다고 알려주는것이다.<br>
이러한 이벤트를 주고받기 위해 **EventEmitter**라는것이 존재한다.<br>
주로 EventEmitter의 **on()**과 **emit()**함수를 사용해 이벤트를 처리하는데<br>
**on()**함수는 이벤트가 전달될 객체에 이벤트 리스너를 설정하고<br>
**emit()**함수는 이벤트를 전달하는 기능을 한다.<br>

보통의 경우 노드에서 제공하는 만들어진 이벤트를 받아 처리(on())하지만, 필요한 경우 이벤트를 만들어 전달해 사용(emit())한다.

calc3.js라는 모듈을 생성한다.

<script src="https://gist.github.com/po9357/02f16f86cf30e1cfa053a549b0f7216f.js"></script>

<br>
그 뒤 해당 모듈을 **require()**로 가져와 사용하면 아래와 같다.

<br>
<script src="https://gist.github.com/po9357/94370844d41095865e1ebc1c6917c430.js"></script>

실행 결과는 아래와 같다.
<img src='/assets/img/runCalc3.JPG' width='500px'>

<br>
모듈 안에 **on()**을 이용해 stop이란 이벤트에 리스너를 등록해 두었다.<br>
모듈을 불러와 **emit()**을 이용해 stop이란 이벤트를 전달해 주었을 때 위에서 등록한 리스너가 실행되는것을 알 수 있다.