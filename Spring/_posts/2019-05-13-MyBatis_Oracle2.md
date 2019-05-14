---
layout: post
title: <p>[Spring] 스프링 MyBatis로 Oracle 연동하기2 </p>
description: >
  MyBatis를 이용해 Spring 환경에서 데이터를 가져오고, 기록하는 법을 알아본다
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, MyBatis, Oracle, 마이바티스]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 이 전 포스팅에서 여러가지 패키지, 인터페이스, 클래스 등을 만들었다.<br>
 이제부터 각각의 역할과 사용법에 대해 알아보자.

## *Mapper.xml

  <img src="/assets/img/spring/oracle3.png">

 우선 src/main/resources/mapper 안에 생성한 BoardMapper.xml 파일을 보자.<br>
 이 파일은 **SQL문**을 작성할 파일이 된다.

 ~~~xml
<bean class="org.mybatis.spring.SqlSessionFactoryBean" id="SqlSessionFactory">
  <property name="dataSource" ref="dataSource" />
  <property value="classpath:mybatis-config.xml" name="configLocation" />
  <property value="classpath:/mapper/*Mapper.xml" name="mapperLocations" />
</bean>
 ~~~

 우리가 이전에 root-context.xml에 MyBatis 연동을 위한 설정을 할때 수정했던 내용이다.<br>
 위 내용을 보면 맨 아래 부분에 **name="mapperLocations"**라 작성된 부분에 클래스 패스로 **/mapper/*Mapper.xml**을 작성해 놓았다. <br>
 이 뜻은 /mapper 안에 있는 *Mapper.xml(뒷 부분이 Mapper.xml로 끝나는 파일)에 대해 매퍼로 지정한다는 의미다.<br>
 따라서 우리가 생성한 BoardMapepr.xml역시 매퍼로 지정된다.

~~~xml
<!-- 전체 내용 조회 -->
<select id="viewAll" resultType="com.my.spring.domain.BoardVO">
  SELECT * FROM board;
</select>
~~~

 BoardMapper.xml 파일에 위의 내용을 작성하자.<br>
 **id**는 해당 SQL문을 호출하기 위한 이름이고 **resultType**은 결과값을 받을 타입을 지정한 것이다.<br>
 결과값으론 이전에 만든 BoardVO타입으로 하되 <u>패키지명까지 정확히</u> 써줘야 한다.<br>
 src/main/resources 안에 만든 **mybatis-config.xml**파일을 이용하면 **alias(별칭)**를 등록할 수 있어<br>
 패키지명까지 쓸 필요없이 간단히 쓸 수 있지만 이번 포스팅에선 그 과정을 생략한다.