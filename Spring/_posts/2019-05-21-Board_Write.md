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
		<input type="text" name="title" style="width: 40%;" placeholder="제목"/>
		<br><br> 
		<textarea id="summernote" name="content"></textarea>
		<input type="submit" value="글 작성" style="float: right;"/>
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