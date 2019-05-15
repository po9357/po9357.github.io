---
layout: post
title: <p>[Spring] JSTL EL(Expression Language)로 화면단에 표현하기</p>
description: >
  Spring - MyBatis를 통해 서버단으로 데이터를 가져와 View로 넘기는것까지 성공했다. 이제 데이터를 화면단에 나타내는 방법에 대해 알아본다.
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, JSP, JSTL, C Tag]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 Model 객체를 이용해 서버단에서 데이터를 속성에 담아 View로 넘겼다.<br>
 View단에서 속성의 Key값으로 불러냈을때 해당 데이터에대한 주소가 출력되었다.<br>
 이제 주소가 아닌 데이터 자체를 표현하는 법을 알아본다.

## EL(Expression Language)
 
 **EL(Expression Language)**이란 말 그대로 표현언어이다. <br>
 기본적으로 ${}의 형식을 사용하고 화면단에 데이터나 파라미터 등을 나타내기 위해 사용한다.<br>
 
## JSTL Core Tag (c 태그)

JSP에서 제공하는 **JSTL(JSP Standard Tag Library)**중 가장 많이 사용되는 태그이다.<br>
JSTL이란 함수, 시간, 문자열 가공, 데이터베이스 엑세스 등을 편하게 해주는 라이브러리다.<br>
**C 태그**는 그 중 가장 많이 사용되는 태그로 반복문, 조건문, 페이지 임포트, 파라미터 관리 등의 기능이 있다.

~~~html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
~~~

C 태그를 불러와 사용하기 위해선 위 코드가 필요하다.<br>
이전에 사용한 board폴더 안에 boardList.jsp파일을 만들고 최상단에 위 코드를 작성한다.<br>

<img src="/assets/img/spring/boardList.png">

이전에 SQL문을 이용해 가져온 데이터를 List에 담아 속성에 저장했다.<br>
List의 내용들을 하나씩 뽑아 사용하기 위해 C 태그의 반복문을 사용한다.

~~~html
<c:forEach items="${viewAll }" var="list">
	<p>제목: ${list.title }</p>
	<p>작성자: ${list.writer }</p>
	<p>내용: ${list.content }</p>
	<hr>
</c:forEach>
~~~

리스트에 담긴 데이터(viewAll)를 하나씩 뽑아 쓰기 위한 코드이다.<br>
대상이 된 데이터는 **items** 속성을 사용하고 사용할 변수는 **var**로 정의한다<br>
이 외에도 일반 반복문과 같이 시작, 끝, 변화값 등을 설정해 사용할 수 있다.

### c:forEach 속성

| 속성 |   설명   |
|:--------:|:--------|
|**var** | 사용할 변수 정의 |
|**begin** | 시작 index 설정 |
|**end**  | 마지막 index 설정 |
|**step** | index 변화값 설정 |
|**items** | 사용할 List 객체 정의 |
|**varStatus** | 진행 중인 index에 대한 정보 |

### varStatus 속성 값

| 값 | 리턴값 |   설명   |
|:--------:|:--------:|:--------|
|**index** | int | 현재 index값 표시 |
|**count** | int | 몇 번째 반복인지에 대해 표시 |
|**first**  | boolean | 첫 번째 index인지에 대해 표시 |
|**last** | boolean | 마지막 index인지에 대해 표시 |