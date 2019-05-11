---
layout: post
title: <p>[Spring] 스프링 컨트롤러(Controller) 사용법 </p>
description: >
  Spring MVC 환경에서 컨트롤러를 사용해 뷰를 컨트롤 해보자
image: /assets/img/programming.jpg
tags: [Spring, MVC, view, controller]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>

 이 전 포스팅을 따라 스프링 프로젝트를 생성했다면 초기에 설정한 패키지 안에 **HomeController.java**파일이 있을것이다.<br>
 스프링 프로젝트 생성시 자동으로 생성되는 컨트롤러인데 이번 포스팅에서 이러한 컨트롤러를 직접 만들고 사용하는 법을 알아본다.

## 컨트롤러(Controller) 생성
 
 <img src="/assets/img/spring/controller.png">

 우선 컨트롤러만 모아둘 **패키지** com.my.spring.controller를 생성한다.<br>
 생성한 패키지 안에 MainController란 이름의 **클래스**를 하나 생성한다.

 <img src="/assets/img/spring/controller3.png">

 MainController에서 클래스 이름 위에 **@Controller** 어노테이션을 추가한다.<br>
 어노테이션을 입력한 뒤 cntl + shift + o 단축키를 이용하면 자동으로 스프링 프레임워크에서 필요한 부분을 임포트한다.

 <img src="/assets/img/spring/controller2.png">

 변경 내용을 저장하면 클래스 파일에 **S마크**가 붙은걸 알 수 있다.<br>
 S마크는 간단히 말해 스프링의 제어를 받는다고 생각하면 된다.<br>
 이제 이 MainController 클래스는 스프링에서 컨트롤러 역할을 하는 클래스가 된 것이다.<br>
 이제 기존에 있던 HomeController 파일은 지우고 MainController클래스에 다음과 같이 작성한다.

 <img src="/assets/img/spring/controller4.png">

 **@RequestMapping**이란 어노테이션이 사용됐다.<br>
 이 어노테이션의 역할은 ()안에 있는 경로와 컨트롤러를 이어주는것(매핑)이다.<br>
 "/"경로로 접속을 시도하면 MainController가 담당을 하고 아래 메소드에 있는 경로들은 "/" 뒤에 붙는 값이 된다.<br>

 <img src="/assets/img/spring/controller5.png">

 위와 같이 작성되어 있다면 "루트/Main" 경로로 접속을 시도하면 해당 컨트롤러가 담당을 하고<br>
 그 아래 main메소드는 "루트/Main/home" 경로를 담당하게 된다.