---
layout: post
title: <p>[Node] 모듈(module)</p>
description: >
  노드에서 모듈에 대해 알아보자.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
<h2> 모듈이란?</h2>

모든 기능을 한 곳에 몰아 넣기보다, 기능별로 구분지어 나눠 관리하면 <br>
다른 곳에서도 해당 기능을 사용할 수 있고, 유지보수 또한 편해진다.

노드에선 기능별로 js파일로 분리시켜 필요한 기능을 불러와 사용하는데<br>
이런 별도로 분리한 파일들을 **모듈(module)**이라 한다.<br>

노드에서 기본으로 제공하는 모듈을 **내장모듈**, <br>
외부에서 다운받은 후 사용 가능한 모듈을 **외장 모듈**이라 한다.

## 모듈의 생성 및 사용

### 모듈 사용법

기본적인 모듈 사용법을 살펴보자.<br>
우선은 모듈을 **require()**함수를 사용하여 불러온다.<br>
쉽게 말하면 기능을 정리해둔 js 파일을 import한다고 생각하면 된다.<br>
그 후 불러온 모듈에서 사용하고자 하는 함수를 불러 사용하면 된다.<br>
이를 코드에서 살펴보면 아래와 같다.

```javascript
var module1 = require('module1');
module1.함수명();
```

### 모듈 만들기

모듈을 만들때에는 **exports** 전역객체를 사용한다.<br>
전역객체인 **exports**에 해당 모듈에 필요한 함수들을 담아 넣으면,<br>
**require()**로 해당 모듈을 불렀을 때 모듈 안의 함수들을 사용할 수 있다.<br>
기본적인 **exports**말고 **module.exports**를 사용하는 방법도 있다.<br>
**module.exports**의 경우는 **exports**처럼 함수 하나하나를 담는게 아닌 객체 자체를 담는다.<br>
코드에서 살펴보면 아래와 같다.
<script src="https://gist.github.com/po9357/f6346534ad426757bf90774848b7a5b4.js"></script>


모듈 사용 예제: 
<script src="https://gist.github.com/po9357/445ebd953cc27cc068da220c3a3cb97e.js"></script>


## 외장 모듈 사용하기

위에서 설명했듯이 외장모듈은 외부에서 다운 후 사용 가능한 모듈이다.<br>
간단하게 다운받는 방법은 **npm패키지**를 이용하는 것이다.<br>
cmd창을 열어 작업중인 폴더로 이동한 후 npm 명령어를 사용해 외장모듈을 다운받을 수 있다.<br>
vs code의 경우 터미널 기능이 있어 별도의 cmd창을 키지 않고 npm 명령어를 사용할 수 있다.
<img src="/assets/img/npmInstall.JPG" width="500px">

다운받은 모듈은 해당 프로젝트내 **node_modules**폴더에 저장된다.<br>
기본적으로 프로젝트별로 모듈을 다운받기 때문에 다른 프로젝트에서 사용하기 위해선 일일이 다운받아야 하는 수고가 있다.<br>
이를 해결하기 위해서 **npm init**명령어를 사용하면 **package.json**파일이 생성되고<br>
이 파일 안에 다운받은 모듈들의 정보가 기록된다.<br>
모듈을 다운받을 시에 **package.json**파일에 기록하기 위해선 **--save**명령어를 덧붙여야 한다.

<img src="/assets/img/npmInit.JPG" width="500px">