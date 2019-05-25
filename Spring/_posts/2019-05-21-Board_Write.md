---
layout: post
title: <p>[Spring] 스프링 게시판 만들기 - 썸머노트(Summernote)를 이용한 글쓰기</p>
description: >
  텍스트 에디터(Summernote)를 이용해 글 작성 기능을 만들어본다. 텍스트 에디터를 이용하면 사용자가 보다 편리하게 글 작성을 할 수 있다. 본 포스팅에선 간편하게 이용할 수 있는 텍스트 에디터인 Summernote를 사용한다. 
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, JSP]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
## 썸머노트(Summernote) 설정하기

 우선 Summernote를 사용하기 위해 https://summernote.org/getting-started/ 에서 파일을 다운받아준다.

 <img src="/assets/img/spring/summernoteDown.png">

 **Download compiled**를 클릭해 다운 뒤 압축을 풀어주면 된다.<br>
 압축을 풀어보면 여러 css, js파일들이 있다. 이 중 우리는 한국어 설정을 위한 js파일만 사용한다.

 <img src="/assets/img/spring/summernoteLang.png">

 압축 푼 폴더를 열고 **dist > lang > summernote-ko-KR.js**파일을 찾아 복사해둔다.

 <img src="/assets/img/spring/resourcesJsSummer.png">
 
 프로젝트에 **webapp/resources/js** 폴더를 만들고 그 안에 summernote-ko-KR.js파일을 넣어준다.<br>
 이제 views/board 폴더에 boardWrite.jsp파일을 만들고 아래 코드를 입력해준다.

~~~html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<!-- include libraries(jQuery, bootstrap) -->
<link href="http://netdna.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.css" rel="stylesheet">
<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.js"></script> 
<script src="http://netdna.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.js"></script> 
<!-- include summernote css/js-->
<link href="https://cdnjs.cloudflare.com/ajax/libs/summernote/0.8.11/summernote-bs4.css" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/summernote/0.8.11/summernote-bs4.js"></script>
<!-- include summernote-ko-KR -->
<script src="/resources/js/summernote-ko-KR.js"></script>
<title>글쓰기</title>

<script>
$(document).ready(function() {
	  $('#summernote').summernote({
 	    	placeholder: 'content',
	        minHeight: 370,
	        maxHeight: null,
	        focus: true, 
	        lang : 'ko-KR'
	  });
	});
</script>
</head>
<body>
<h2 style="text-align: center;">글 작성</h2><br><br><br>

<div style="width: 60%; margin: auto;">
	<form method="post" action="/write">
		<input type="text" name="writer" style="width: 20%;" placeholder="작성자"/><br>
		<input type="text" name="title" style="width: 40%;" placeholder="제목"/>
		<br><br> 
		<textarea id="summernote" name="content"></textarea>
		<input id="subBtn" type="button" value="글 작성" style="float: right;" onclick="goWrite(this.form)"/>
	</form>
</div>

</body>
</html>
~~~

MainController에  아래 메소드를 작성한다.

~~~java
@GetMapping("write")
public String boardWrite() {
  return "board/boardWrite";
}
~~~

서버를 더블클릭하고 아래 사진과 같이 path를 **/**로 바꿔준 뒤 실행한다.

<img src="/assets/img/spring/serverPath.png">

이제 서버를 키고 실행시키면 루트 경로가 "**localhost:포트번호/**"가 된다.

<img src="/assets/img/spring/write.png">

"**localhost:포트번호/write**"로 접속시 위와 같은 화면이 뜨면 성공이다.

## 데이터베이스에 insert 하기

화면단은 완성되었고 이제 글을 입력받아 서버로 넘긴 뒤 DB에 기록해야한다.<br>

~~~html
<script>
function goWrite(frm) {
	var title = frm.title.value;
	var writer = frm.writer.value;
	var content = frm.content.value;
	
	if (title.trim() == ''){
		alert("제목을 입력해주세요");
		return false;
	}
	if (writer.trim() == ''){
		alert("작성자를 입력해주세요");
		return false;
	}
	if (content.trim() == ''){
		alert("내용을 입력해주세요");
		return false;
	}
	frm.submit();
}
</script>	
~~~

글 작성내용에 빈칸이 없게끔 확인하는 코드이다.<br>
**header** 태그 안이나 **body** 태그 맨 아래 위 코드를 작성해준다.

~~~sql
CREATE SEQUENCE  "BOARD_SEQ"  
MINVALUE 1 MAXVALUE 9999999999999999999999999999 
INCREMENT BY 1 START WITH 1 NOCACHE;
~~~

데이터베이스에서 인덱스를 쉽게 사용하기 위한 시퀀스를 생성해준다.<br>
더미데이터가 있을시 삭제하거나 **START WITH** 뒤의 숫자를 바꿔 인덱스가 겹치지 않게한다.

~~~xml
<!-- 게시판 insert -->
<insert id="insertBoard" parameterType="com.my.spring.domain.BoardVO">

	<selectKey keyProperty="seq" order="BEFORE" resultType="int">
		SELECT BOARD_SEQ.NEXTVAL FROM DUAL
	</selectKey>
	INSERT INTO BOARD (seq, writer, title, content)
				VALUES (#{seq}, #{writer}, #{title}, #{content})
</insert>
~~~

**BoardMapper.xml** 파일에 위 sql문을 작성해준다.<br>
여기서 **selectKey**란 해당 insert문 실행 전이나 후에<br>
selectKey 안의 내용을 실행시킨 후 해당 내용을 반환해준다.<br>
즉 다음 시퀀스 값을 **select**한 후 해당 값을 **BoardVO** 안에 넣어주는 기능을 한다.

~~~java
// 게시물 작성
public int insertBoard(BoardVO vo);
~~~

**BoardMapper**와 **BoardService** 인터페이스에 위 메소드를 추가한다.

~~~java
@Override
public int insertBoard(BoardVO vo) {
	return mapper.insertBoard(vo);
}
~~~

**BoardServiceImpl** 클래스에 메소드를 Override 해준다.

~~~java
@PostMapping("write")
public String write(BoardVO vo) {
	boardService.insertBoard(vo);
	return "redirect: /detail?seq="+ vo.getSeq();
}
~~~

마지막으로 **Controller**에 위 내용을 입력해준다.<br>
이미 **GetMapping**으로 'write' 경로를 사용했지만 GET방식만 받으므로<br>
같은 경로로 GET, POST 각각 따로 처리할 수 있다.<br>
글을 쓴 뒤 **redirect**를 이용해 해당 글 번호로 이동하게끔 구현한다.<br>

~~~html
<input type="button" value="글쓰기" style="float: right;" onclick="location.href='/write'">
~~~

**boardList.jsp**에 글쓰기 버튼을 만들어 준 뒤 서버를 실행시키고 글작성을 해본다.

<img src="/assets/img/spring/krBrk.png">

한글로 작성시 위와같이 한글이 깨지는 경우가 있다. <br>
POST방식으로 데이터 전송시 문제가 생기는데 <br>
이 경우는 Spring에서 **web.xml**에 필터를 등록해주면 해결된다.

~~~xml
<filter>
<filter-name>encodingFilter</filter-name>
<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<init-param>
	<param-name>encoding</param-name>
	<param-value>utf-8</param-value>
</init-param>   
</filter>

<filter-mapping>
	<filter-name>encodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
~~~

**src/main/webapp/WEB-INF** 폴더 **web.xml**에 위 내용을 작성해주면 된다.<br>
다시 실행 뒤 테스트를 해보면

<img src="/assets/img/spring/writeTest.png">
<img src="/assets/img/spring/writeResult.png">
<img src="/assets/img/spring/writeList.png">

위와 같이 정상적으로 작동되는걸 알 수 있다.