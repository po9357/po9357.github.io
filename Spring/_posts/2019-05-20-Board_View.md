---
layout: post
title: <p>[Spring] 스프링 게시판 만들기 - 상세보기(조회수)</p>
description: >
  게시판 목록을 출력하는 방법까지 알아보았다. 이제 글작성 기능을 구현하는 방법에 대해 알아본다.
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, JSP]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
 게시판 목록에서 특정 글을 보기 위해선 해당 글을 식별할 수 있는 값이 필요하다.<br>
 
## SQL문으로 데이터 가져오기

 <img src="/assets/img/spring/oracle7.png">
 
 본 포스팅에서 사용하는 테이블 구조는 위와 같다.<br>
 PK(기본키)로 지정된 컬럼(SEQ)를 이용해 게시글을 식별한다.<br>

~~~html
<tr>
  <td>${list.seq }</td>
  <a href='detail?seq=${list.seq }'><td>${list.title }</td></a>
  <td>${list.writer }</td>
  <td><fmt:formatDate value="${list.regdate }" pattern="yyyy.MM.dd"/> </td>
  <td>${list.cnt }</td>
</tr>
~~~
 
 우선 **boardList.jsp** 파일 일부를 위와 같이 수정해준다.<br>
 a태그로 링크를 걸어 seq 파라미터를 GET방식으로 넘기는 코드이다.<br>
 이렇게 넘긴 파라미터를 컨트롤러에서 받아 사용한다.

~~~xml
<!-- 글 상세조회 -->
<select id="viewDetail" resultType="com.my.spring.domain.BoardVO" parameterType="int">
  SELECT * FROM BOARD WHERE SEQ = #{seq}
</select>
~~~

**BoardMapper.xml**에 상세 조회를 하기 위한 sql문을 작성해준다.

~~~java
// 게시글 상세보기
public BoardVO viewDetail(int seq);
~~~

**BoardMapper.java**와 **BoardService.java** 인터페이스에 위 메소드를 작성한다.

~~~java
@Override
public BoardVO viewDetail(int seq) {
  return mapper.viewDetail(seq);
}
~~~

**BoardServiceImpl.java** 클래스에 메소드를 Override해준다.

~~~java
@GetMapping("detail")
public String viewDetail(Model model, 
                        @RequestParam("seq")int seq
                        // BoardVO vo
                        ) {
  
  model.addAttribute("board", boardService.viewDetail(seq));
  //model.addAttribute("board", boardService.viewDetail(vo.getSeq()));

  return "board/viewDetail";
}
~~~

**MainController.java**에 위 메소드를 작성해준다.<br>
**@GetMapping**어노테이션은 GET방식의 url요청만 매핑해주는 어노테이션이다.<br>
**@RequestParam**어노테이션은 View단에서 seq라는 이름으로 넘긴 파라미터를 받아준다.<br>
@RequestParam으로 받는 방식도 있고, **VO타입**을 지정해주면 그에 맞는 파라미터를 스프링에서 매치해 넣어주기때문에 위의 주석부분처럼 사용하는 방법도 있다.

## 화면단에 출력하기

~~~html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>글 상세보기</title>
</head>
<style>
	h2 { text-align: center;}
  table { width: 100%;}
  textarea { width: 100%;}
 	#outter {
		display: block;
		width: 30%;
		margin: auto;
	}
</style>
<script>
	function goList() {
		location.href='board';
	}
</script>
<body>

<h2>게시판</h2>
<br><br><br>
<div id="outter">
	<table border="1">
		<tr>
			<td>제목: ${board.title }</td>
		</tr>
		<tr>
			<td>
				작성자: ${board.writer }
				<span style="float: right;"><fmt:formatDate value="${board.regdate }" pattern="yyyy.MM.dd"/></span>
			</td>
		</tr>
		<tr>
			<td><textarea rows="10" readonly>${board.content }</textarea></td>
		</tr>
	</table>
	<input type="button" value="글 목록" style="float: right;" onclick="goList()"> 
</div>
</body>
</html>
~~~

views폴더에 **viewDetail.jsp**파일을 만들고 위 내용을 작성해준다.<br>
이제 프로젝트를 실행시켜본다. 

<img src="/assets/img/spring/boardList3.png">

'루트/board'로 들어가보면 제목부분에 링크가 걸려있다.

<img src="/assets/img/spring/viewDetail.png">

제목을 클릭하면 상세 페이지가 나오고 글 목록 버튼을 누르면 다시 목록으로 돌아온다.

## 조회수 올리기

이제 제목 클릭시 조회수를 1씩 올리는 작업을 해보자.

~~~xml
<!-- 조회수 +1 -->
<update id="plusCnt" parameterType="int">
  UPDATE BOARD SET CNT = CNT + 1 WHERE SEQ = #{seq}
</update>
~~~

**BoardMapper.xml**에 sql문을 작성해준다.

~~~java
// 조회수 +1
public boolean plusCnt(int seq);
~~~

**BoardMapper.java**와 **BoardService.java** 인터페이스에 위 메소드를 작성해준다.

~~~java
@Override
public boolean plusCnt(int seq) {
  return mapper.plusCnt(seq);
}
~~~

**BoardServiceImpl.java** 클래스에 위 메소드를 Override 해준다.

~~~java
@GetMapping("detail")
public String viewDetail(Model model, @RequestParam("seq")int seq) {
  
  model.addAttribute("board", boardService.viewDetail(seq));
  
  //조회수 +1
  boardService.plusCnt(seq);
  
  return "board/viewDetail";
}
~~~

이제 **MainController.java** 컨트롤러에서 조회수를 올리는 sql문을 호출해주면 끝이다.<br>
프로젝트를 실행하여 글 상세보기로 간 후 목록으로 돌아오면 조회수가 올라간것을 볼 수 있다.