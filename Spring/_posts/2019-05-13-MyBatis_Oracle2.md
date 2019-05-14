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

## Mapper 인터페이스

~~~xml
<mapper namespace="com.my.spring.mapper.BoardMapper">
~~~

 BoardMapper.xml을 보면 상단에 위와 같이 작성되어 있을것이다.<br>
 SQL문을 작성한 매퍼와 namespace로 등록한 인터페이스를 연동한다는 뜻이다.<br>
 해당 인터페이스로 이동해 아래와 같은 코드를 작성해준다.

~~~java
public List<BoardVO> viewAll();
~~~
 
 위에서 작성한 SQL문의 결과값이 여러개일 수 있으므로 <br>
 BoardVO타입만 담을 수 있는 List타입으로 메소드를 정의하고<br>
 매퍼파일에서 불러올 <u>SQL문의 id</u>를 메소드 이름으로 입력해준다.

## Service 영역

 com.my.spring.service 안에 생성해둔 BoardService 인터페이스로 이동한 후 아래 내용을 작성해준다.

~~~java
public List<BoardVO> viewAll();
~~~

BoardMapper인터페이스에서 작성한 내용과 동일한 내용이다.<br>
BoardMapper에서 사용한 **viewAll**이란 이름은 매퍼파일의 <u>id값과 매칭</u>하기 위함이었다.<br>
하지만 서비스 영역은 해당 SQL문을 이용해 <u>어떠한 기능을 하는가</u>에 대해 작명해주면 된다.<br>
따라서 꼭 **viewAll**로 이름을 정의할 필요는 없지만 본 포스팅에선 편의를 위해 동일하게 작성한다.

~~~java
@Service
public class BoardServiceImpl implements BoardService {
	
	@Autowired
	private BoardMapper mapper;
	
	@Override
	public List<BoardVO> viewAll() {
		return mapper.viewAll();
	}

}
~~~

BoardService를 상속받는BoardServiceImpl로 이동한 후 위의 코드를 작성한다.<br>
**@Service** 어노테이션은 해당 클래스가 구현된 Service란것을 알리기 위한것이다.<br>
**@Override** 어노테이션은 부모 객체에 있는 메소드를 Override했단 뜻이다.<br>
**@Autowired** 어노테이션은 의존성 주입을 위한 것이다. <br>
BoardMapper **인터페이스**타입의 mapper 변수를 선언하였다.<br>
기본적으로 인터페이스는 객체를 생성할 수 없다. 즉 **new** 키워드를 사용한 객체생성이 불가능하다.<br>
@Autowired를 사용하면 **Spring Framework**가 해당 인터페이스(BoardMapper)를 참조하고 <br>
해당 인터페이스와 연동된 매퍼(BoardMapper.xml)을 참고해 자동으로 객체를 생성해 mapper란 변수에 넣어준다.<br>
이러한 작업을 <a href="https://po9357.github.io/spring/2019-05-06-DependencyInjection/">의존성 주입(Dependency Injection)</a>이라 한다.<br>
