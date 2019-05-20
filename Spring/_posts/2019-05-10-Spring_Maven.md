---
layout: post
title: <p>[Spring] 스프링 기초 - Maven을 통한 프로젝트 설정 </p>
description: >
  Spring 기본 설정을 위해 사용하는 Maven을 사용하는법을 알아보고 
image: /assets/img/programming.jpg
tags: [Maven, Mybatis, 스프링, 스프링 설정, Spring 설정, Spring mvc]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 우리가 자바를 사용할때 필요한 라이브러리가 있으면 불러와 사용해야 한다.<br>
 보통은 jar파일로 라이브러리를 받아 빌드패스를 설정한 후에 사용할 수 있는데<br>
 Spring MVC 작업환경에선 이러한 수고를 덜 수 있다.

## 메이븐 사용하기(Maven)
 
 <img src="/assets/img/spring/pom.png">

 우리가 전 포스팅에서 만든 프로젝트를 보면 **pom.xml**이란 파일이 생성되어 있다.<br>
 **메이븐(Maven)**이란 자바 프로젝트의 환경 설정 등을 도와주는 Tool이다.<br>
 메이븐은 pom.xml을 참조해 설정을 도와주기 때문에 우리는 pom.xml을 잘 작성해주면 된다.

 <img src="/assets/img/spring/pom2.png">

 우선 pom.xml을 살펴보면 여러가지 프로젝트 설정에 관한 내용이 작성되어있다.<br>
 상단에 보면 java-version, springframework-version등 사용 버전이 작성되어 있고<br>
 아래로 내리다 보면 &lt;dependency&gt;에 감싸져 groupId, artifactId, version 등이 작성되어 있는걸 볼 수 있다.

 우선 자바와 스프링 버전을 변경해준다.

 ```xml
<properties>
  <java-version>1.6</java-version>
  <org.springframework-version>3.1.1.RELEASE</org.springframework-version>
  <org.aspectj-version>1.6.10</org.aspectj-version>
  <org.slf4j-version>1.6.6</org.slf4j-version>
</properties>
 ```

 이 부분을 아래와 같이 바꿔준다.

 ~~~xml
<properties>
  <java-version>1.8</java-version>
  <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
  <org.aspectj-version>1.6.10</org.aspectj-version>
  <org.slf4j-version>1.6.6</org.slf4j-version>
</properties>
 ~~~
 
 사용하는 자바는 1.8버전으로, 스프링은 5.0.7버전을 사용한다.<br>
 아래로 내려보면 최하단 부분에 

 ~~~xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.5.1</version>
    <configuration>
        <source>1.6</source>
        <target>1.6</target>
        <compilerArgument>-Xlint:all</compilerArgument>
        <showWarnings>true</showWarnings>
        <showDeprecation>true</showDeprecation>
    </configuration>
</plugin>
~~~

이 부분에서 1.6이라 작성된 부분도 1.8로 바꿔준다. 이 부분은 컴파일 버전을 뜻한다.<br>
이제 우리가 필요한 라이브러리를 찾아 알맞게 xml파일에 작성해주면 된다.<br>
우리는 추후 Mybatis를 이용해 DB연동을 할 예정으로 Mybatis에 대한 라이브러리를 추가한다.

<img src="/assets/img/spring/mavenRepository.png">

메이븐 레파지토리(https://mvnrepository.com/)사이트에 접속한다.<br>
그 후 필요한 라이브러리(추후에 사용될 Mybatis)를 검색한다. 

<img src="/assets/img/spring/mybatis.png">

검색 후 가장 상단에 있는 MyBatis 클릭한다.

<img src="/assets/img/spring/mybatis2.png">

위와 같은 화면이 나오는데, MyBatis의 여러 버전들과 사용량, 날짜 등이 나와있다.<br>
이번 프로젝트에선 3.4.6 버전을 사용한다.

<img src="/assets/img/spring/mybatis3.png">

3.4.6을 클릭하면 위와 같은 화면이 나온다.<br>
하단에 보면 Maven에 해당되는 xml이 작성되어 있다.<br>
그 부분을 클릭하면 드래그한것 처럼 변하고 자동으로 클립보드에 복사된다.

<img src="/assets/img/spring/mybatis5.png">

복사한 부분을 pom.xml에 적당한 곳에 붙여놓기 한 후 저장을 하면 라이브러리를 다운받고

<img src="/assets/img/spring/mybatis4.png" width="30%">

MyBatis 라이브러리가 정상적으로 설치된것을 알 수 있다.<br>
위에서 스프링 버전을 5.0.7로 바꿔 스프링에 관한 라이브러리도 5.0.7버전으로 바뀐걸 알 수 있다.

~~~xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.2</version>
</dependency>

<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-jdbc</artifactId>
  <version>${org.springframework-version}</version>
</dependency>
~~~

MyBatis를 사용해 DB연동을 하기 위해 몇 가지 라이브러리를 더 추가한다.<br>
위의 내용을 입력하면 MyBatis Spring과 Spring jdbc라이브러리가 추가된다.