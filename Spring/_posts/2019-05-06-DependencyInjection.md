---
layout: post
title: <p>[Spring] 의존성 주입(Dependency Injection) </p>
description: >
  의존성 주입이란? 스프링을 본격적으로 시작하기 전에 스프링을 이해하기 위한 필수 요소인<br>
  의존성 주입에 대해 알아보자.
image: /assets/img/programming.jpg
tags: [의존성 주입, DI, 의존성]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

 이 전 포스팅에서 간단하게 객체지향 개념에 대해 알아보았다.<br>
 간단히 말해 프로그램을 객체들의 모임으로 생각해<br>
 상속을 이용해 객체간의 관계를 규정하는것이다.

## 의존성 주입(Dependency Injection)이란?
 
 객체지향 개념을 기본으로 객체 생성을 사용자가 아닌 외부에서 알아서 주입해주는것이다.<br>
 Spring Framework는 **어노테이션(Annotation)**을 사용한다.<br>
 어노테이션이란 쉽게 말해 **기능이 있는 주석**이라 볼 수 있다. 

 스프링에서 의존성 주입에 사용되는 어노테이션은 주로 **Autowired, Inject, Resource 등**이 있다.<br>
 의존성 주입의 개념만을 설명하기 위한 포스트이므로, 각 어노테이션에 대한 설명은 건너 뛴다.<br>

 이 전 포스트에서 마린을 생성하는 코드를 가상으로 작성해보면 아래와 같다
 
 ~~~java
 Marine mr1 = new Marine();
 Marine mr2 = new Marine();
 Marine mr3 = new Marine();
 ~~~

 Marine 타입의 변수를 선언하고 Marine 클래스(Unit Interface를 상속받았다 가정) 객체를 생성하는것이다.<br>
 내가 생성하고자 하는 객체를 찾아 **new 키워드**를 사용하여 객체를 생성한다.<br>
 이 코드는 일반적으로 자바에서 사용하는 객체 생성 코드이다.<br>

 **의존성 주입**은 위에서 말했듯이 객체 생성을 외부에서 알아서 해주는것이다.<br>

 ~~~java
 @Autowired
 Marine mr1;
 ~~~

 위와 같이 작성하면 **@Autowired** 어노테이션을 참고해 Marine타입의 객체를 찾아 주입해준다.<br>
 사용자는 생성하고자 하는 타입과 변수만 정의하고 어노테이션을 사용하면 Spring Framework가 알아서 객체를 생성한다.<br>
 위 과정에서 객체생성의 주체가 **사용자**가 아닌 **컨테이너(Spring Framework 등)**가 되기 때문에<br>
 **제어의 역전(Inversion of Controls)**이 일어나고 그 방법론 중 하나가 **의존성 주입(Dependency Injection)**이다.

## 의존성 주입을 사용하는 이유?
 
 의존성 주입은 컴파일시가 아닌 실행시에 이루어지기 때문에 모듈들간 **결합도**를 낮출 수 있다.<br>
 또한 코드의 **재사용**을 높여 코드 수정없이 여러곳에서 사용 가능하다.<br>

 또 다른 이유로 **관심의 분리(Seperation of Concern)**를 들 수 있다. <br>
 즉, '객체 생성 등의 자잘한 작업은 컨테이너가 할테니 사용자는 할 일에 집중해라'라는 개념이다.<br>
 컨테이너가 알아서 자잘한 작업을 처리해주기 때문에 사용자는 그러한 작업에 신경을 쓸 필요없이<br>
 오롯이 본인이 작업하는 코드만 작성하는 편의성을 제공해준다. 


### 참조

 https://okky.kr/article/362415<br>
 https://ko.wikipedia.org/