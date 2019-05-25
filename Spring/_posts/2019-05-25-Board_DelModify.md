---
layout: post
title: <p>[Spring] 스프링 게시판 만들기 - 글 삭제 / 수정</p>
description: >
  글 목록과 작성 기능을 구현했다. 이제 글 수정과 삭제 작업만 하면 기본적인 CRUD 작업은 끝이난다. Summernote를 이용한 글 수정과 간단한 글 삭제 기능을 구현해보자.
image: /assets/img/programming.jpg
tags: [Spring, Spring MVC, JSP]
comments: true
---
<head>
  <link rel="stylesheet" type="text/css" href="../../assets/css/obsidian.css" />
</head>
 
## 글 삭제하기

 글 삭제는 간단하게 구현할 수 있다.<br>
 글 상세보기를 구현할때 사용한 인덱스 값을 이용해 DB에서 삭제처리만 해주면 된다.

~~~html
<div style="float: right;">
	<input type="button" value="수정" onclick="location.href='modify?seq=${board.seq}'">
	<input type="button" value="삭제" onclick="del(${board.seq})">
	<input type="button" value="글 목록" onclick="location.href='board';">
</div>
~~~

글 삭제 / 수정 버튼 추가를 위해 **viewDetail.jsp** 파일을 살짝 수정해준다.

~~~html
<script>
	function del(seq) {
		var chk = confirm("정말 삭제하시겠습니까?");
		if (chk) {
			location.href='delete?seq='+seq;
		}
	}	
</script>
~~~

삭제 확인을 위한 스크립트도 작성해준다.

~~~xml
<!-- 게시글 삭제 -->
<delete id="deleteBoard" parameterType="int">
	DELETE FROM BOARD WHERE SEQ = #{seq}
</delete>
~~~

**BoardMapper.xml** 파일에 위 SQL문을 작성해준다.

~~~java
// 게시물 삭제
public boolean deleteBoard(int seq);
~~~
**BoardService** 와 **BoardMapper** 인터페이스에 위 메소드를 추가한다.

~~~java
@Override
public boolean deleteBoard(int seq) {
	return mapper.deleteBoard(seq);
}
~~~
**BoardServiceImpl** 클래스에 위 메소드를 추가한다.

~~~java
@GetMapping("delete")
public String delete(@RequestParam("seq")int seq) {
	boardService.deleteBoard(seq);
	return "redirect: /board";
}
~~~

마지막으로 **MainController**에 위 메소드를 추가한 뒤 실행한다.

<img src="/assets/img/spring/confirm.png">

글 상세페이지에서 삭제 버튼을 누르면 위와 같이 확인창이 뜨고 확인을 누르면 글 삭제후 목록으로 이동한다.

## Summernote로 글 수정하기

글을 수정할때 데이터를 **DB**에서 다시 가져오는 방법과 **front단**에서 서버로 넘기는 방법이 있다.<br>
본 포스팅에선 미리 작성해둔 SQL로 DB에서 다시 가져오는 방법을 사용한다.

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
	  // Summernote에 글 내용 추가하는 코드
	  $("#summernote").summernote('code',  '${board.content}');
	});
</script>
</head>
<body>
<h2 style="text-align: center;">글 수정</h2><br><br><br>

<div style="width: 60%; margin: auto;">
	<form method="post" action="/modify" >
		<input type="hidden" name="seq" value="${board.seq}">
		<input type="text" name="writer" style="width: 20%;" placeholder="작성자" value="${board.writer }" readonly/><br>
		<input type="text" name="title" style="width: 40%;" placeholder="제목" value="${board.title }"/>
		<br><br> 
		<textarea id="summernote" name="content"></textarea>
		<input id="subBtn" type="button" value="글 수정" style="float: right;" onclick="goModify(this.form)"/>
	</form>
</div>
<script>
function goModify(frm) {
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
</body>
</html>
~~~

글 수정을 위해 **boardModify.jsp**파일을 생성하고 위 코드를 작성한다.

~~~xml
<!-- 게시글 수정 -->
<update id="updateBoard" parameterType="com.my.spring.domain.BoardVO">
	UPDATE BOARD SET TITLE = #{title}, CONTENT = #{content} WHERE SEQ = #{seq}
</update>
~~~

**BoardMapper.xml** 파일에 위 sql문을 작성한다.

~~~java
// 게시물 수정
public boolean updateBoard(BoardVO vo);
~~~

**BoardMapper** 와 **BoardService** 인터페이스에 위 메소드를 추가한다.

~~~java
@Override
public boolean updateBoard(BoardVO vo) {
	return mapper.updateBoard(vo);
}
~~~

**BoardServiceImpl** 클래스에 위 메소드를 추가한다.

~~~java
@GetMapping("modify")
public String modify(@RequestParam("seq")int seq, Model model) {
	model.addAttribute("board", boardService.viewDetail(seq));
	return "board/boardModify";
}

@PostMapping("modify")
public String modify(BoardVO vo) {
	boardService.updateBoard(vo);
	return "redirect: /detail?seq="+ vo.getSeq();
}
~~~

**MainController**에 위 메소드를 추가하면 끝난다.<br>
이제 프로젝트를 실행하여 테스트 해본다.

<img src="/assets/img/spring/modify.png">
<img src="/assets/img/spring/modDone.png">

수정 버튼을 누르면 summernote가 활성화된 수저 페이지가 나오고<br>
수정을 하면 수정 후 상세페이지로 이동하게 된다.