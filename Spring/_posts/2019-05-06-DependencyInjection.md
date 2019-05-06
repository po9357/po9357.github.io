---
layout: post
title: <p>[Spring] 의존성 주입(Dependency Injection)</p>
description: >
  스프링을 본격적으로 시작하기 전에 스프링을 이해하기 위한 필수 요소인<br>
  의존성 주입에 대해 알아보자.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

## 의존성 주입이란?

 스프링을 제대로 이해하기 위해선 객체지향 개념부터 확실히 알 필요가 있다.<br>
 Spring MVC가 돌아가는 방식은 '어노테이션'을 이용한 의존성 주입을 이용하기 떄문에<br>
 겉으로 보아선 이해하기 힘들다.<br>

## 객체지향 개념

 <img src="/assets/img/marine.png">
 <img src="/assets/img/scv.png"><br>

 위 사진은 스타크래프트란 게임의 '마린'과 'SCV' 유닛의 상태창이다.<br>
 각각의 버튼들마다 각기 다른 기능을 담고있다.<br>

 두 유닛을 보면 상위 3가지 버튼은 동일하고 나머진 다른 버튼임을 알 수 있다.<br>
 스타크래프트에서 대부분의 유닛들은 모두 저 3가지 버튼을 가지고 있는데, 순서대로 '이동, 정지, 공격'이다.<br>
 <br>
 이를 객체지향형 프로그램에 대입해 보자.<br>
 간단한 정리를 해보면

 1. 모든 유닛들은 '이동, 정지, 공격'의 **공통 기능**을 가지고 있다.
 2. 각 유닛 종류마다 **공통 기능**을 제외한 **고유한 기능**을 가지고 있다.

 <img src="/assets/img/marine.png">
 
 빨간 테두리 부분이 **공통 기능**, 초록 테두리가 **고유 기능**이 된다.<br>
 객체지향언어(Java)에서 공통 기능을 가진 부분은 **인터페이스**로 정의하면 된다.<br>
 고유 기능을 가진 부분은 해당하는 **클래스**를 생성하면 된다.