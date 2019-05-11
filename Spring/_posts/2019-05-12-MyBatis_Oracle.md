---
layout: post
title: <p>[Spring] 스프링 MyBatis로 Oracle 연동하기 </p>
description: >
  MyBatis를 이용해 Spring 환경에서 DB 연동하는법을 알아본다
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, MyBatis, Oracle, 마이바티스]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 게시판을 만들고 사용하기 위해선 DB연동이 필수적이다.<br>
 본 포스팅에선 Oracle 11g를 사용하고 MyBatis를 이용한 연동을 알아본다.

## 라이브러리 추가
 
 이 전 포스팅에서 Maven을 통해 라이브러리를 추가하는법을 알아보았다.<br>
 MyBatis, MyBatis-spring, Spring-jdbc 라이브러리를 추가했었는데, 필요한 라이브러리 몇 가지를 더 추가한다.

 ~~~xml
<!-- https://mvnrepository.com/artifact/commons-dbcp/commons-dbcp -->
<dependency>
    <groupId>commons-dbcp</groupId>
    <artifactId>commons-dbcp</artifactId>
    <version>1.4</version>
</dependency>
  
<!-- https://mvnrepository.com/artifact/com.oracle/ojdbc6 -->
<dependency>
  <groupId>com.oracle</groupId>
  <artifactId>ojdbc6</artifactId>
  <version>11.2.0.3</version>
</dependency>
 ~~~

 Oracle과 연결하기위한 ojdbc와 커넥션풀 라이브러리를 추가한다.<br>
 **커넥션풀**이란 db 커넥션을 미리 만들어놓고 요청이 들어오면 커넥션을 하나씩 배정해<br>
 DB에 접속하는 시간을 줄여주는 역할을 한다.

 **ojdbc**는 Maven에서 제공하지 않는 라이브러리다.<br>
 따라서 위와같이 dependency만 작성해주면 제대로 ojdbc를 다운받지 못한다.

 ~~~xml
<properties>
  <java-version>1.8</java-version>
  <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
  <org.aspectj-version>1.6.10</org.aspectj-version>
  <org.slf4j-version>1.6.6</org.slf4j-version>
</properties>

<repositories>
  <!-- 오라클 JDBC라이브러리 관리 사이트 -->
  <repository>
    <id>oracle</id>
    <name>Oracle JDBC Repository</name>
    <url>http://repo.spring.io/plugins-release/</url>
  </repository>
</repositories>
 ~~~

 pom.xml 상단부분에 Oracle에 관한 repository를 추가해주면<br>
 Maven이 해당 레파지토리를 참고해 ojdbc를 다운받는다.

<img src="/assets/img/spring/oracle.png">

 MyBatis에 관한 설정을 하기 위해 src/main/resources안에 **mybatis-config.xml**파일을 생성한다.<br>
 mybatis-config.xml안에 아래와 같이 작성해준다.

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
~~~

Oracle DB와 연동하기 위해 **root-context.xml**파일에 다음 내용을 추가한다.

~~~xml
<bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" id="dataSource">
  <property value="oracle.jdbc.driver.OracleDriver" name="driverClassName" />
  <property value="jdbc:oracle:thin:@localhost:1521:xe" name="url" />
  <!-- 오라클 사용자 이름 -->
  <property value="MYSTUDY" name="username" />
  <!-- 오라클 사용자 비밀번호 -->
  <property value="mystudypw" name="password" />
</bean>

<bean class="org.mybatis.spring.SqlSessionFactoryBean" id="SqlSessionFactory">
  <property name="dataSource" ref="dataSource" />
  <property value="classpath:mybatis-config.xml" name="configLocation" />
  <property value="classpath:/mapper/*Mapper.xml" name="mapperLocations" />
</bean>

<mybatis-spring:scan base-package="com.my.spring.mapper" />
~~~

오라클 사용자 이름과 비밀번호는 본인이 생성한 DB 사용자로 설정하면 된다.<br>
위의 &lt;bean&gt; 안의 내용은 오라클 DB와 연결하는 내용이고<br>
아래 &lt;bean&gt; 안의 내용은 연결한 오라클 DB에 관한 설정 파일들에 관한 내용이다.

내용을 제대로 추가했다면 아마 오류표시가 뜰 것이다. <br>
이유는 네임스페이스가 제대로 추가되지 않았기 때문인데 Mybatis-spring, Spring-jdbc 라이브러리가 제대로 설치되었다면 아래와 같이 네임스페이스를 추가할 수 있다.

<img src="/assets/img/spring/oracle2.png" width="70%">

root-context.xml을 키고 하단에 Namespaces탭을 클릭하면 위와같은 화면이 나온다.<br>
여기서 mybatis-spring을 체크하고 저장하면 된다. 만약 mybatis-spring이 없다면 <br>
pom.xml에 아래 내용을 추가한 뒤에 시도하면 된다.

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
  <version>5.0.7.RELEASE</version>
</dependency>
~~~

 이제 기본적인 설정에 관한 내용은 끝났다.<br>
 이제 나올 내용들은 조금 복잡해 보일 수 있다. 천천히 따라온다면 크게 어렵지 않을것이다.

<img src="/assets/img/spring/oracle4.png">

 우선 com.my.spring.mapper 패키지(root-context.xml에 정의된 경로)를 생성한 후 BoardMapper.java 클래스를 만든다.

<img src="/assets/img/spring/oracle3.png">

 src/main/resources 안에 mapper폴더를 만들고 그 안에 BoardMapper.xml파일을 생성한다.<br>
 이 경로 또한 root-context.xml안에 정의했다.

 ~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//ibatis.apache.org//DTD Mapper 3.0//EN" "http://ibatis.apache.org/dtd/ibatis-3-mapper.dtd">
<mapper namespace="com.my.spring.mapper.BoardMapper">
		
	
</mapper>
~~~

BoardMapper.xml안에 위 내용을 작성해준다.<br>
중요한 점은 namespace 내용에 위에 생성한 클래스 경로를 정확하게 작성해 주어야 한다.

<img src="/assets/img/spring/oracle5.png">

다음으로 com.my.spring.service 패키지와 com.my.spring.service.impl 패키지를 생성한다.<br>
service 패키지엔 BoardService **인터페이스**를 생성하고 service.impl 패키지엔<br> 
생성한 BoardService 인터페이스를 상속받는 BoardServiceImpl **클래스**를 생성한다.

<img src="/assets/img/spring/oracle6.png">

com.my.spring.domain 패키지를 생성하고 BoardVO 클래스를 만들어준다.<br>
BoardVO엔 Oracle에서 생성한 테이블에 대한 VO가 작성된다.<br>
오라클에서 BOARD란 테이블을 다음과 같이 생성하였다.

<img src="/assets/img/spring/oracle7.png">

아래 DDL문을 복사해 사용해도 된다.

~~~DDL
CREATE TABLE "BOARD" 
(	"SEQ" NUMBER(5,0), 
"TITLE" VARCHAR2(200 BYTE), 
"WRITER" VARCHAR2(20 BYTE), 
"CONTENT" VARCHAR2(2000 BYTE), 
"REGDATE" DATE DEFAULT SYSDATE, 
"CNT" NUMBER(5,0) DEFAULT 0, 
PRIMARY KEY ("SEQ")
) ;
~~~

BoardVO에 테이블에 맞게 아래 내용을 작성한다.

~~~java
package com.my.spring.domain;

import java.util.Date;

public class BoardVO {

	int seq, cnt;
	String title, writer, content;
	Date regdate;
	
}
~~~

위와 같이 작성해준 뒤

<img src="/assets/img/spring/oracle10.png">

우클릭 -> Source -> Generate Getters and Setters... 클릭

<img src="/assets/img/spring/oracle9.png" width="50%">

Select All 클릭한 뒤 OK를 클릭하면 자동으로 Getter와 Setter가 만들어진다.<br>
"lombok"이란 라이브러리를 사용하면 이런 과정없이 어노테이션만 작성하면 되지만<br>
본 포스팅에선 다루지 않는다.