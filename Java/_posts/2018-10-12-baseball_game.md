---
layout: post
title: <p>[Java] 자바로 만든 간단한 야구 게임<p>
description: >
  자바 연습겸 만들어본 야구게임
image: /assets/img/baseball.jpg
---
 배열도 배우기 전에 반복문과 조건문을 이용해서<br/>
 간단한 야구게임을 구현해보았다.


## 야구게임 전체 코드 

<script src="https://gist.github.com/po9357/f03c0d121ba23c17342ad7cd854ca183.js"></script>

### 진행 방식
각 자리수가 겹치지 않게 4자리 정수를생성한 뒤 <br>
각 자리수를 추출하여 저장한다   <br>
<br>
그 후 4자리 정수를 입력받아 <br>
입력받은 수의 각 자리수와<br>
생성된 수의 각 자리수를 비교하여<br>
STRIKE, BALL을 판정 후 콘솔에 출력한다.<br>
<img src="/assets/img/baseball1.JPG">

이 과정을 반복하여 정답이 나오면 반복문을 빠져나오고 종료된다.
<img src="/assets/img/baseball2.JPG">
