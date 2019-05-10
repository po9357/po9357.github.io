---
layout: post
title: <p>[Spring] 스프링 MVC 프로젝트 Maven 설정 </p>
description: >
  Spring MVC 환경설정을 위해 사용하는 Maven에 대해 알아본다.
image: /assets/img/programming.jpg
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 우리가 자바를 사용할때 필요한 라이브러리가 있으면 불러와 사용해야 한다.<br>
 보통은 jar파일로 라이브러리를 받아 빌드패스를 설정한 후에 사용할 수 있는데<br>
 Spring MVC 작업환경에선 이러한 수고를 덜 수 있다.

## 메이븐 사용하기(Maven)
 
 우리가 전 포스팅에서 만든 프로젝트를 보면 **pom.xml**이란 파일이 생성되어 있다.<br>
 **메이븐(Maven)**이란 자바 프로젝트의 환경 설정등을 도와주는 Tool이다.<br>
 메이븐 pom.xml을 참조해 설정을 도와주기 때문에 우리는 pom.xml을 잘 작성해주면 된다.

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

이 부분에서 1.6이라 작성된 부분도 1.8로 바꿔준다. 이 부분은 컴파일 버전을 뜻한다.