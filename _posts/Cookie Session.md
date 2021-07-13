# Cookie Session

> ### 쿠키 : 서버측 정보가 사용자 컴퓨터에 저장되는것

ex) 자동 로그인 체크박스 통해서 쿠키 저장 동의를 받아 저장합니다.(통행증과 비슷)

![](D:\git blog 정리\쿠키 예시.png)

* 쿠키를 통해 정보가 유출될 가능성이 있다. 보안이 약하다.
* 서버측에서 사용자가 필요한 정보를 담아서 사용자 컴퓨터로 보내줍니다.

* 쿠키는 해당 클라이언트당 한개 만 줍니다.

* 쿠키는 웹서버에서 만들어 클라이언트에 전달합니다.

* 클라이언트는 쿠키가 저장되어 있는지 쿠키저장소를 열어서 확인 합니다.



1. 로그인 -> 자동 로그인 체크 ->  확인 클라이언트 pc에 쿠키 저장

쿠키생성

```
Cookie cookie = new Cookie("","");
쿠키는 기본생성자가 없어서 생성자안에 키값과 밸류값 적어야합니다.
```

2. 클라이언트 checkbox input에 value 값에 1을적고 submit으로 보내면 값이 넘어온다.
3. 체크박스에 체크 안되어있으면 null 체크 되어있으면 1이 보내져서 체크박스 확인이 가능하다.

* proc 파일

```java
<%
	request.setCharacterEncoding("euc-kr");
	//아이디 저장 체크박스가 체크 되었는지 판단여부
	String save = request.getParameter("save");
	//아이디 값을 저장
	String id = request.getParameter("id");
	//체크 되었는지를 판단
	if(save!=null){  //아이디 저장이 눌렷다면
		//쿠키를 사용하려면 쿠키 클래스를 생성해줘야함
		Cookie cookie = new Cookie("id", id); //1번째 String 에는 키값을 2번째는 value 적어줌
		//쿠키 유효시간 설정
		cookie.setMaxAge(60*10); //10분간 유효
		
		//사용자에게 쿠키 값 넘겨줌
		response.addCookie(cookie);
		out.write("쿠키생성 완료");	
	}
	
	%>
```



* LoginForm 파일

```java
	<%
	//사용자 컴퓨터의 쿠키 저장소로부터 쿠키 값 읽어들임 -몇개인지 모르기에 배열로 읽음
	Cookie[] cookies = request.getCookies();
	String id = "";
	//쿠키 값이 없을수도 있기에 null 처리를 해줍니다
	if (cookies != null) {
		for (int i = 0; i < cookies.length; i++) {
			//쿠키에 적혀있는 key값 으로 찾습니다.
			if (cookies[i].getName().equals("id")) {
		id = cookies[i].getValue();  //getname이 아니라 getvalue로 key value 가져올땐 getkey get value
		break;//원하는 데이터를 찾앗기에 반복문 빠져 나옴
			}
		}
	}
	%>
```

