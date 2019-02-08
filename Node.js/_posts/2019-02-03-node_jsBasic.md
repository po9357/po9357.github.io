---
layout: post
title: <p>[Node] Node사용을 위한 기본 자바스크립트</p>
description: >
  노드를 사용하기 위해선 자바스크립트 활용이 필수다.<br> 
  이를 위한 기본 자바스크립트 기능 몇 가지를 알아본다.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

<h2>자바스크립트의 객체와 함수</h2>

### 객체 생성

자바스크립트는 기본적으로 변수 선언시 타입을 지정하지 않는다.<br>
**var** 키워드를 사용해 모든 변수를 생성할 수 있으며, <br>
자바나 C언어처럼 변수의 타입지정 없이 사용한다.

그렇기 때문에 객체 선언도 마찬가지로 **var** 키워드를 사용하고, 중괄호를 이용한다.<br>
생성한 객체에 접근할 때에는 대괄호([])를 이용하거나 점(.) 뒤에 속성명을 붙여 접근할 수 있다.

<br>
<script src="https://gist.github.com/po9357/ae647b23f39948db358c9587ad2ce9e7.js"></script>

### 함수 생성

변수 선언과 마찬가지로 함수 생성시에도 자바스크립트는 타입을 선언하지 않는다.<br>
자바의 경우 해당 함수의 리턴값에 해당하는 타입을 함수 생성시 선언한다.<br>
자바 스크립트의 경우 변수를 타입별로 명확히 구분하지 않기 때문에<br>
함수도 마찬가지로 타입선언을 하지 않는다. 그렇다고 리턴값이 없는것은 아니다.<br>
자바스크립트의 경우 **return**키워드를 사용하지 않는 함수라도 리턴은 무조건 일어나게 된다.

<br>
<script src="https://gist.github.com/po9357/31fb1c5906a64ee16bf99ea98345a56f.js"></script>


객체 안에서 함수를 선언: 

<br>
<script src="https://gist.github.com/po9357/458612e94e55f5bf5edd249f7bcd4764.js"></script>



## 배열

여러 데이터를 한 곳에 차례로 저장할 수 있는 배열이다. <br>
자바와 같이 인덱스를 이용해 원하는 순번의 데이터에 접근할 수 있다.<br>
배열 안에 기본적인 데이터 타입들은 물론 객체도 저장할 수 있고, 저장시에는 **push**명령어를 사용한다.

배열 안의 데이터를 하나하나 조회할땐 반복문을 사용한다. <br>
자바와 동일한 for문을 사용해도 되고 자바스크립트의 forEach를 사용해도 된다.

<br>
<script src="https://gist.github.com/po9357/2cdb755a075e1ea8b35b186deafd2f58.js"></script>


### 배열 추가, 삭제

배열에 데이터를 추가 혹은 삭제할때 사용할 수 있는 함수엔 여러가지가 있다.

```javascript
push(Object) // 배열 끝에 하나의 데이터 추가
pop() // 배열 끝에 있는 데이터 삭제
unshift() // 매열 앞에 데이터 추가
shift() // 배열 앞에 있는 데이터 삭제
splice(index, removeCount, [Object]) // 여러 데이터를 추가 혹은 삭제
slice(index, copyCount) // 여러 데이터를 잘라 새로운 배열객체 생성
```

<br>
<script src="https://gist.github.com/po9357/355f29ff8ed40acaf40e06cf281cab24.js"></script>


## 콜백 함수

콜백함수란 함수 안에서 다른 함수를 파라미터로 전달해 처리할때 호출된 다른 함수를 말한다.<br>
쉽게 말해 함수에서 호출한 함수라 할 수 있다.

콜백함수는 보통 비동기처리에서 많이 쓰인다. 동기 처리 중에 시간이 걸리는 함수를 비동기처리하면<br>
함수를 실행하는 동안 동기 처리도 진행되기 때문에 효율적으로 사용할 수 있다.

<br>
<script src="https://gist.github.com/po9357/40762fa15c7546873c052eb9daae2421.js"></script>