# Model1 Model2

* #### model 1 방식과 model2 방식의 차이

<img src="https://github.com/dsds601/java/blob/main/img/Servlet.png?raw=true" style="zoom: 50%;" >

* Client가 웹서버에 접속하여 request를 보내면 JSP **model1** 에서는 **Controller 부분까지도 JSP가 담당**을하엿는데 **model2 방식**에서는 **Servlet(java파일) 이 담당**을하게된다. Model클래스(DAO)클래스에서 DB서버에 접속하여 데이터를 가져와 Servlet에서 결과를 넘기기 위해 **RequestDispatcher**를 통해 View에 데이터를 보내 화면을 만들어 웹페이지에 뿌려준다.
  * **RequestDispatcher란?**
    * 현재 request에 담긴 정보를 저장하고 있다가 다음페이지에도 해당 정보를 볼수 있게 저장하는 기능
      request - respnse 단계에서 forward 와 sendredirect 방식이 있는데 forward는 정보를 가지고 넘겨주는 기능이 있지만 sendredirect 방식은 단순 페이지 호출 기능만 가지고 있습니다.하지만 RequestDispatcher 없이 forward 하면 파라미터정보는 넘어가지만 별도의 파라미터를 저장 안하면 정보가 소실됩니다. 그래서 RequestDispatcher 선언후 request에 attribute를 저장 후 forward 시켜야 합니다.



* <b>순서</b>
  * Model1 방식
    * Client -> View -> Model -> DB ->View -> Client
  * Model2 방식
    * Client -> Contoller -> Model ->DB -> Controller ->View -> Client
* <b>Model2 방식에 장점</b>
  * View안에 자바코드가 섞여 있을경우 프론트와 백엔드에 구분이 모호해지고 역활을 분담하기 힘들어진다.
  * Model2에서는 프론트와 백엔드 부분을 구분하여 편하게 작업을 할수있다.
  * 방문자수가 적어 리뉴얼이 많지 않는 사이트에 경우 Model1 방식으로 처리하여도 된다.



* **서블릿 만드는 방법**

  1. 간단 방법

     1. 이클립스에서 서블릿 코드생성
     2. 서블릿(Servlet) 코드 작성
     3. URL 과 서블릿 **매핑 하기**  @어노테이션
     4. 매핑 결과 View로 넘기기
     5. 실행 후 웹브라우저에서 결과 확인<br>

     * 서블릿 변경이 많지 않으면 이 방법이 편하다.

     

  2. web.xml 이용한 방법

     1. 등록하고자 하는 서블릿의 이름과 등록하고자 하는 서블릿 구현 클래스 지정
     2. servlet-mapping을 통해 name과 url-pattern을 지정

     ```java
     <servlet>
       <servlet-name>ServletTest</servlet-name> //servlet 이름 명시
       <servlet-class>servlet.HelloWorld</servlet-class> //패키지.클래스 이름을 등록
       </servlet>
       
       <servlet-mapping>
       <servlet-name>ServletTest</servlet-name> //servlet의 name을 매핑
       <url-pattern>/hello.do</url-pattern>  //uri 패턴 반드시 필요!! (주소 뒤에 값이라서)
       </servlet-mapping>
     
     ```

     * servlet.HelloWorld 를 만들고  url이 hello.do 가 들어오면 HelloWorld 클래스가 실행된다.
     * 서블릿이 변경이 많이되고 많이 사용되야 한다면 web.xml을 이용한다.

