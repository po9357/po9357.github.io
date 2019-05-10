---
layout: post
title: <p>[Spring] 스프링 MVC 프로젝트 만들기 </p>
description: >
  기본적인 Spring MVC 프로젝트 생성하기
image: /assets/img/programming.jpg
tags: [Spring, spring mvc, spring 프로젝트, spring 만들기]

comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

 <img src="/assets/img/icon/spring/STS.png" width="80%">

 본 포스팅에선 **STS(Spring Tool Suite)**를 이용한다.<br>
 이클립스 내 **Eclipce Marketplace**에서 STS를 다운 받거나<br>
 **STS 사이트** (https://spring.io/tools3/sts/all)에서 다운받을 수 있다.<br>
 다운 받는 과정은 스킵하고 프로젝트 생성 과정만을 다룬다.

## 스프링 MVC 프로젝트 만들기

 <img src="/assets/img/spring/SP1.png" width="70%">

 우선 자바에서 프로젝트를 만들듯 **Spring Legacy Project**를 생성한다.<br>

 <img src="/assets/img/spring/SP2.png" width="50%">

 **프로젝트 이름**을 정하고(본 포스팅에선 Ex01) **Spring MVC Project**를 선택한 후 Next를 누른다.<br>

 <img src="/assets/img/spring/SP3.png" width="70%">
 
 그 다음으로 top-level package 경로를 설정해주어야 하는데<br>
 주의할 점은 반드시 **3단계** 이상으로 구성되어야 한다.<br>
 본 포스팅에선 com.my.spring으로 설정하였다.

 <img src="/assets/img/spring/SP4.png" width="70%">

 성공적으로 프로젝트가 만들어졌다면 일단 프로젝트를 우클릭하여 프로젝트를 실행시켜본다.<br>
 Spring을 공부하는 사람이라면 서버 생성 및 구동은 알고있을거라 생각하여 그 과정은 생략했다.<br>
 본 프로젝트에선 Apache Tomcat 9.0 버전을 사용해 서버를 구동하였다.

 <img src="/assets/img/spring/SP5.png">

 위와 같은 화면이 나온다면 Spring MVC Project를 생성하고 서버 연동까지 성공한것이다.<br>
 다음 포스팅에선 여기서 만든 프로젝트로 게시판을 만들기 위한 설정에 대해 알아본다.
 