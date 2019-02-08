---
layout: post
title: <p>[Node] 프로토타입(Prototype)</p>
description: >
  노드를 사용하기 위한 자바스크립트 중 프로토타입(prototype)에 대해 알아본다.
image: /assets/img/programming.jpg
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

<h2>프로토 타입(Prototype)</h2>

자바스크립트도 자바와 마찬가지로 **객체지향 언어**이다. 하지만 자바스크립트는 자바의 **Class**라는 개념이 없다.
Class가 없으면 상속 또한 불가능하기 때문에 **Prototype**을 이용해 상속과 비슷한 기능을 할 수 있다.

<br>
<script src="https://gist.github.com/po9357/df8f6fc49e06a538a8f1cb3b371bfa03.js"></script>

위의 경우 User1과 User2 모두 같은 name과 age속성을 가지고 있는데, <br>
각각 객체를 생성해 메모리에는 똑같은 데이터가 따로따로 할당된다.

**프로토타입(Prototype)**을 사용하면 불필요한 메모리 할당 없이 한 번 할당된 메모리에서 꺼내쓸 수 있다.

<br>
<script src="https://gist.github.com/po9357/b6f6b7d60157db4500b5310702cf44a5.js"></script>

이 경우 함수를 선언한 뒤에 prototype이란 곳에 name과 age속성을 정의했다.<br>
그 뒤 User1과 User2 각각 객체를 생성하였는데<br> 
이 경우에는 User함수의 prototype이란 곳을 공유하게 된다.<br>

### 생성자

자바스크립트에서 **new** 키워드를 붙여 호출하는 함수는 객체 생성을 위한 함수이다.<br>
Array, Object 등 객체를 만들기 위한 이러한 함수를 생성자(Constructor)라고 한다.<br>
위 예제에서 User1과 User2가 new User()를 통해 객체를 생성했듯이 User() 또한 생성자가 된다.

이러한 생성자는 **Prototype Object**를 가지고 있다.<br>
prototype이라는 속성을 통해 이 Prototype Object에 접근할 수 있다.<br>
prototype 속성 안에는 **constuctor**와 **__proto__**를 가지고 있다. 

**constructor**는 Prototype Object와 같이 생성된 함수를 가르킨다.<br>
**__proto__**는 Prototype Link라 하는데, 객체 생성시 사용한 **생성자**의 Prototype Object를 가르킨다. 

따라서 생성자의 **prototype**안에 데이터, 객체 혹은 함수 등을 정의해놓으면 <br>
생성자이용해 생성한 객체에서는 **Prototype Link**를 이용해 **생성자.prototype**에 접근할 수 있다.