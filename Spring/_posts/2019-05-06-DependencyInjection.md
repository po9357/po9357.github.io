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
 <img src="/assets/img/SCV.png">
 위 사진은 스타크래프트란 게임의 '마린'과 'SCV' 유닛의 상태창이다.<br>
 각각의 버튼들마다 각기 다른 기능을 담고있다.<br>

 두 유닛을 보면 상위 3가지 버튼은 동일하고 나머진 다른 버튼임을 알 수 있다.
 스타크래프트에서 대부분의 유닛들은 모두 저 3가지 버튼을 가지고 있는데, 순서대로 '이동, 정지, 공격'이다.

 