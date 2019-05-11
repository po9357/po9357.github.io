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
 "루트/"경로로 접속을 시도하면 MainController가 담당을 하고 아래 메소드에 있는 경로들은 "루트/" 뒤에 붙는 값이 된다.<br>

 <img src="/assets/img/spring/controller5.png">

 위와 같이 작성되어 있다면 "루트/Main" 경로로 접속을 시도하면 해당 컨트롤러가 담당을 하고<br>
 그 아래 main메소드는 "루트/Main/home" 경로를 담당하게 된다.

 메소드 타입을 String으로 정의하고 "home"을 리턴했다.<br>
 이 "home"값은 리턴이 되면 스프링이 알아서 분석하고 자동으로 src/main/webapp/WEB-INF/views 안에 있는 home.jsp와 연결해준다. 이에 관한 설정은 **servlet-context.xml**에 작성되어 있다.

 <img src="/assets/img/spring/controller6.png">

servlet-context.xml을 보면 name="prefix"로 된 부분에 "**/WEB-INF/views**"가 적혀있고<br>
name="suffix"로 된 부분엔 "**.jsp**"가 작성되어 있다.<br>
이 설정에 따라 "home"을 리턴하면 home의 앞엔 **prefix**부분이, 뒤엔 **suffix**부분이 자동으로 붙어 /WEB-INF/views/home.jsp가 완성되는 것이다.

 <img src="/assets/img/spring/controller7.png">

그 아래 **context:component-scan** 부분은 스프링이 탐색하는 범위를 뜻한다.<br>
기본으로 com.my.spring으로 설정되어 있어 그 아래 작성된 com.my.spring.controller패키지도 탐색 범위에 포함된다.<br>
만약 MainController가 com.my.spring과 전혀 다른 패키지에 작성되어 있다면 S마크도 붙지 않고 컨트롤러 역할도 하지 못하게 된다.

이제 컨트롤러가 정상작동하는지 확인하기 위해 프로젝트를 실행시켜본다.<br>
프로젝트 우클릭 Run As -> Run on Server로 실행해도 되고<br>
서버만 실행시킨 뒤 localhos:포트번호/루트 경로로 접속해도 된다.

 <img src="/assets/img/spring/controller8.png">

프로젝트를 처음 생성하고 실행했을때와 비슷한 이런 화면이 나온다면 성공이다.<br>
이제부터 본격적으로 스프링을 이용해 게시판 만드는법을 알아본다.