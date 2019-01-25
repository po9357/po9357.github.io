---
layout: post
title: <p>[Java] 자바로 만든 간단한 야구 게임<p>
description: >
  자바 연습겸 만들어본 야구게임
image: /assets/img/baseball.jpg
---
 배열도 배우기 전에 반복문과 조건문을 이용해서<br/>
 간단한 야구게임을 구현해보았다.


## 야구게임 전체 코드 

```java
import java.util.*;

public class My_Ex_baseball {
	public static void main(String[] args) {
		
		Scanner sc = new Scanner(System.in);
		String com;
		String input;
		
		
		char chc1;
		char chc2;
		char chc3;
		char chc4;
		while(true) {
			System.out.println("중복된 수가 없는 4자리 정수를 생성중입니다.(1000 ~ 9999)");
			com = (int)(Math.random()*9000+1000)+"";
		 	chc1 = com.charAt(0);
		 	chc2 = com.charAt(1);
		 	chc3 = com.charAt(2);
		 	chc4 = com.charAt(3);
		 	
		 	if (!(chc1==chc2 || chc1==chc3 || chc1==chc4 || chc2==chc3 || chc2==chc4 || chc3==chc4))
		 		break;
		}
		
		while(true){
			System.out.println("1000 ~ 9999사이의 4자리 정수를 입력하여 주십시오.");
			input = sc.nextLine();
			char chi1 = input.charAt(0);
			char chi2 = input.charAt(1);
			char chi3 = input.charAt(2);
			char chi4 = input.charAt(3);
		
			if (com.equals(input)) {
				System.out.println("정답입니다!!");
				break;
			}
			
			if (chi1=='1' && chi2=='1' && chi3=='1' && chi4=='1') {
				System.out.println(com);
			}
			
			if (chi1 == chc1) {
				System.out.println("Strike");
			}else if (chi1==chc2 || chi1==chc3 || chi1==chc4) {
				System.out.println("ball");
			}
			
			if (chi2 == chc2) {
				System.out.println("Strike");
			}else if (chi2==chc1 || chi2==chc3 || chi2==chc4) {
				System.out.println("ball");
			}
			
			if (chi3 == chc3) {
				System.out.println("Strike");
			}else if (chi3==chc1 || chi3==chc2 || chi3==chc4) {
				System.out.println("ball");
			}
			
			if (chi4 == chc4) {
				System.out.println("Strike");
			}else if (chi4==chc1 || chi4==chc2 || chi4==chc3) {
				System.out.println("ball");
			}
		
		}
		
	}
}
```

### 진행 방식
각 자리수가 겹치지 않게 4자리 정수를생성한 뒤 <br>
각 자리수를 추출하여 저장한다   <br>
<br>
그 후 4자리 정수를 입력받아 <br>
입력받은 수의 각 자리수와<br>
생성된 수의 각 자리수를 비교하여<br>
STRIKE, BALL을 판정 후 콘솔에 출력한다.<br>
<img src="/assets/img/baseball1.JPG">

이 과정을 반복하여 정답이 나오면 반복문을 빠져나오고 종료된다.
<img src="/assets/img/baseball2.JPG">
