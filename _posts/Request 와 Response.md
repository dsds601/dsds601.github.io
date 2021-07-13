# Request 와 Response

* ### Request

|         Request          |      웹 브라우저(클라이언트)를 통해 서버에 요청하는것       |
| :----------------------: | :---------------------------------------------------------: |
|                          | 웹브라우저에서 서버에 요청하는 정보가 Request 객체에 담긴다 |
|                          |    JSP 컨테이너가 서블릿으로 변환될때 자동으로 생성된다     |
|        **method**        |                                                             |
| getParmeter(String name) |      name에 해당하는 요청한 파라미터 값을 리턴받는다.       |
|       getSession()       |                  Session 객체를 리턴받는다                  |
|     getRequestURL()      |                  요청 URL값을 리턴받는다.                   |



* ### Response

|         Response         |         서버에서 웹브라우저(클라이언트)에 응답하는것         |
| :----------------------: | :----------------------------------------------------------: |
|                          | 클라이언트의 요청에 응답하는것이 Response이고 , 응답의 정보는 Response객체에 담기게 된다. |
|                          | Request는 헤더의 값을 읽어오는 반면 , Response는 헤더에 값을 추가하고 , 리다이렉트 하는것이 가능하다. |
|        **method**        |                                                              |
| addCookie(Cookie cookie) |                      cookie를 추가한다.                      |
| sendRedirect(String URL) |                 해당 URL로 리다이렉트 한다.                  |

