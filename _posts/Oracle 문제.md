---
layout: post
title:  "Oracle 숫자함수"
summary: "Oracle 숫자함수"
author: GH
date: '2021-07-05 14:35:23 +0530'
category: Oracle
thumbnail: /i.imgur.com/FdJ8Xbd.png
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /blog/categories/Oracle-Oracle 숫자함수/
usemathjax: true
---



 문제
---



> ### employee 테이블에서 모든 컬럼의 데이터를 검색하면?

* **SELCET**         원하는 컬럼명       **from**    **테이블명**;

  * **SELECT** emp_no , emp_name , dep_no ,jikup ,salary , hire_date , jumin_num , phone from **employee**;

  * **select**       *        from    employee;   ***=>전체 컬럼 출력***  

    * 전체 출력시 CREATE  할때 정한 순서대로 다 나옵니다.

      

    * 보기 편한 방법

    ```sql
    SELECT emp_no ,
           emp_name , 
           dep_no ,
           jikup ,
           salary , 
           hire_date , 
           jumin_num , 
           phonefrom employee;
    ```

    

---

### employee 테이블에서 emp_no , emp_name , jikup ,salary , hire_date  from 컬럼의 데이터만 검색하고 싶다면

```sql
SELECT emp_no , 
       emp_name , 
       jikup ,
       salary , 
       hire_date 
  FROM employee;
```

> ###  employee 테이블에서 emp_no , emp_name , jikup ,salary , hire_date 을 검색하면서 별칭, alias (헤데)를 사원번호 , 직원명 , 직급 , 연봉 , 입사일로 하고 연봉에 '만원' 이라는 문자를 붙여 검색하면?

**주의** 

* 헤더는 데이터가 아니라서 (*)싱글쿼터를 안쓰고 더블 쿼터(**) 를 사용합니다

```sql
SELECT emp_no AS "사원번호" , 
       emp_name AS "직원명" , 
       jikup AS "직급" , 
       salary AS "연봉" , 
       hire_date AS "입사일" 
  FROM employee;
```



* **as** 를 생략해도 헤더는 바뀝니다.

```sql
SELECT emp_no "사원번호" , 
       emp_name "직원명" , 
       jikup "직급" , 
       salary "연봉" , 
       hire_date "입사일" 
  FROM employee;
```



* (**) 더블쿼터를 생략하여도 헤더는 바뀝니다.

* **주의**  단 공백이 필요할땐 더블쿼터를 사용해야 합니다.

```sql
SELECT emp_no 사원번호 사원 번호->에러 "사원 번호"->에러x , 
       emp_name 직원 , 
       jikup 직급 , 
       salary 연봉 , 
       hire_date 입사일 
  FROM employee;
```



* 만원  salary 옆에 붙이기

* **주의**  싱글쿼터로 가능하고 더블쿼더 불가능
  * 싱글쿼터는 문자 **문자 데이터** 사용할때만 사용합니다.
  * 오라클에서 연결연산자는 || 입니다. + 는 오로지 사칙연산으로만 사용합니다.

```sql
SELECT emp_no "사원번호" , 
       emp_name "직원명" , 
       jikup "직급" , 
       salary||'만원' "연봉" , 
       hire_date "입사일" 
  FROM employee;
```

---

* ### employee 테이블에서 직원명 , 직급 , 연봉 , 세금을 검색하면? (세금은 연봉의 12%)

```sql
SELECT emp_name AS "직원명" ,
       jikup AS "직급" ,
       salary * 0.12 AS "세금" 
  FROM employee;
```

* **NUMBER형** 컬럼 자료형에만 연산이 가능합니다.



>  employee 테이블에서 직원명 , 직급 , 연봉 , 세금 , 실수령액을 검색하면 ? (세금은 12%)

```sql
SELECT emp_name AS "직원명" ,
       jikup AS "직급" ,
       salary * 0.12 AS "세금" ,
       salary * 0.88 AS "실수령액" 
  FROM employee;
```

* 실제 데이터에는 실수령액이 없으므로 **가상**으로 보여주는것 입니다 . 
  * 실제 바뀌기 위해서는 UPDATE를 사용해야 합니다.



---

* ### employee 테이블에서 직급을 중복 없이 검색하면?

```sql
SELECT DISTINCT jikup 
  FROM employe e
  --함수에 형태로 검색 
SELECT DISTINCT(jikup) 
  FROM employee;

SELECT UNIQUE(jikup) 
  FROM employee;
```

* unique도 괄호를 제거하면 여러개 사용 가능합니다.
* distinct는 여러개 사용 못한다 한번 선언하고 뒤에 여러 컬럼명을 입력해야 한다.
  * **컬럼을 여러개 사용**한거라면 여러 컬럼을 하나로보고 그중 중복되지 않게 검색이 됩니다.
  * select distinct jikup distinct dep_no form employee;

 

> ### employee 테이블에서 부서번호와 직급을 중복 없이 검색하면?

```sql
    SELECT      distinct dep_no , jikup 	FROM      	employee;  
```



> ### employee 테이블에서 연봉이 3000이상인 직원을 검색하면 ?

* **where** 절 이용!
  * where 는 행을 골라 내는 키워드!
  * **<주의>**  **모든 행** 이란 말이 나오면 **WHER** 절이 없습니다.

```sql
SELECT * FROM employee WHERE salary >= 3000;
```



> ### employee 테이블에서 연봉 오름차순으로 검색하면?

* order by 키워드 사용

  * 행의 순서를 오름차순으로  바꿔주는 키워드 asc  (오름차순 작->큰)
  * asc 는 생략 가능

  ```sql
  select * from employee order by salary asc;select * from employee order by salary;
  ```

  * 행의 순서를 내침차순으로 바꿔주는 키워드 desc (내림차순 큰 -> 작)

  ```sql
  select * from employee order by salary desc;
  ```

  * 컬럼번호 을 통해서 차순을 바꿀수 있습니다.

  ```sql
  select * from employee oreder by 5 ;
  ```

  * 컬럼번호를  통해 내림차순

  ```sql
  select * from employee order by 5 desc  ;
  ```

> #### employee 테이블에서 부선호를 오름차순 유지하며 연봉을 내림차순으로 검색하면



*   **, (컴마)**로 원하는 컬럼명 asc      **or**    desc 입력

```sql
select * from employee order by dep_no asc , salary desc;
```

* **주의**      **asc** 가 생략되고 desc가 뒤에 있는경우 desc가 전체를 영향주는것 같아 헷갈릴수 있다.

```sql
select * from employee oredr by dep_no , salary desc;
```



> #### employee 테이블에서 직급이 높은 순서 나열해서 검색하면?

```sql
select * from employee order by jikup desc;select * from employee order by jikup asc;
```

* 위 답은 모두 잘못된 것 입니다.
* 직급 순서는 인간이 생각하는 기준이지 오라클의 단순한 정렬 개념과 다릅니다. 추후 조건문을 통해 정렬의 기준을 바꿔야 합니다.

**정답**

```sql
--직급이 높은 순서
select * from employee order by decode(jikup,'사장',1,'부장',2,'과장',3,'대리',4);
--직급이 낮은 순서
select * from employee order by decode(jikup,'사장',1,'부장',2,'과장',3,'대리',4) desc;
--직급이 낮고 봉급이 높은 순서
select * from employee order by decode(jikup,'사장',1,'부장',2,'과장',3,'대리',4) desc , salary desc;
```

**주의**

* 고객의 요구 사항을 보고  select 문을 작성하는것도 중요하지만 이미 작성된 select 문을 보고 고객의 요구 사항을 알아내는것도 중요하다



> #### employee 테이블에서 부장만 검색하면?

```sql
select * from employee where jikup = '부장';
```

* 오라클은   **''  =  ''**  비교 연산자 입니다.



> #### employee 테이블에서 20번 부서의 과장만 검색하면?

* 오라클에 **and &&**    연산자는  **and** 입니다.

```sql
SELECT * 
  FROM employee 
 WHERE dep_no = 20 
       AND jikup = '과장';
```



>####  employee 테이블에서 20번 부서이거나  과장인 사람을 검색하면?

* 오라클에 **or ||**    연산자는  **or** 입니다.

```sql
SELECT * 
  FROM employee 
 WHERE dep_no = 20 
       OR jikup = '과장';
```



> #### employee 테이블에서 과장 중에 연봉 3400 이상을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE jikup = '과장' 
       AND salary >= 3400;
```



> #### employee 테이블중에서 실수령 액이 4000만원 이상  받는 직원을 검색하면 ? (단 세금이 12%)

```sql
SELECT * 
  FROM employee 
 WHERE salary * 0.88 >= 4000; 
실수령액 >= 4000만원
```



> #### employee 테이블에서 세금을 제일 많이 내는 직원 순서로 나열하면서 부서번호가 내림차순이면서 사장이 아닌 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE jikup != '사장' 
ORDER BY salary*0.12 desc ,
       dep_no desc;
```



> #### employee 테이블에서 20번 부서 중에 연봉 2000~3000 사이 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE dep_no = 20 
       AND salary >=2000 
       AND salary <=3000;

SELECT * 
  FROM employee 
 WHERE dep_no = 20 
       AND salary BETWEEN 2000 AND 3000;
```

* **between** 문법 통해 조금 더 가독성 좋게 표현 할수 있다.
  * 최소값 이상 최대값 이하 표현



> ### employee 테이블에서 직속 상관이 없는 직원을 검색하면?

```sql
select * from employee where mgr_emp_no is null;      
```

* null 값 찾을땐  **=**  쓸 수 없다. oracle에서 = 찾으려면 실존하는 데이터만 가능하다
  * null 값을 찾기 위해서는 **is null**  통해서만 가능합니다.



> #### employee 테이블에서 직속 상관이 있는 직원을 검색하면?

```sql
select * from employee where mgr_emp_no is not null ;	
```

* null 값 찾을때 **=** 쓸 수 없듯이   **!=**  도 사용 못합니다.

  * **!=** 과 동일하게 사용하기 위해선 **is not** 수식어를 사용합니다.

  

> #### employee 테이블에서 [최소연봉] , [최대연봉] , [평균 연봉] , [연봉총합] ,[총 인원수]을 검색해서 출력하면?

```sql
SELECT min(salary) AS"최소연봉" ,
       max(salary) AS"최대연봉",
       avg(salary) AS "평균연봉",
       sum (salary) AS "연봉총합",
       count (salary) AS "연봉총합" 
  FROM employee;
```

* 그룹 함수 or 통계함수 [그룹함수들은 주로 group by와 같이 많이 사용된다.] **NULL** 값은 **제외** 하고 계산합니다.
  * min(컬럼명) : 최소값 찾는 oracle 함수 
  * max(컬럼명) : 최대값 찾는 oracl 함수 
  * avg(컬럼명) : 평균값 찾는 oracle 함수  => 더할때 null값이 있다면 빠지고 나눗셈할때 갯수에서도 빠지게된다.
  * sum(컬럼명) :총 합계를 찾는 oracle 함수
  * count(컬럼명) : 컬럼 안 총 갯수를 리턴하는 orclae 함수  => null값이 있으면 null값은 빼고 값을 셈니다.  모든 행에 갯수를 찾으려면 * 입력 하면 됩니다! **count  ( * ) **  -> 총 행의 갯수 리턴
    * **count(*) count(컬럼) 비교!**

> #### 아래 SQL 구문은 무슨 문제 인가?
>
> ```sql
> select count(emp_no) from customer
> ```

* 담당 직원 있는 고객의 명수



> #### 고객을 담당하고 있는 직원의 명수는?

```sql
select count ( distinct  emp_no) from customer;
```





> #### 부하직원이 있는 직원의 명수는?

```sql
select count(mgr_emp_no) from employee;
```



> #### 부하직원이 있는 직원의 명수는?

```sql
select count(distinct mgr_emp_no) from employee
```

* 직속상관들은 겹치기에 **distinct**로 중복을 제거해서 사용 합니다.



> #### employee 테이블에서 [직원번호] , [직원명], [생일월-생일일] 검색해서 출력하면?

```sql
SELECT emp_no "직원번호" , 
       emp_name "직원명" , 
       substr(jumin_num,3,2)||'-'||substr(jumin_num,5,2) "생일월일" 
  FROM employee
```

oracle 스트링 객체 자르는 함수 : **substr** (컬럼명 , 시작 지점 , 시작지점으로부터 갯수)



> #### customer 테이블에서 모든 컬럼, 모든 행을 검색해서 출력하면?
>
> 단 , 주민번호는 901225-2****  형태로 출력하세요

```sql
SELECT cus_no ,
       cus_name ,
       tel_num ,
       substr(jumin_num,1,6)||'-'||substr(jumin_num,7,1)||'*****' ,
       emp_no 
  FROM customer
```



> #### customer 테이블에서 고객번호, 고객명 , 담당직원번호 를 출력하면?
>
> 단 담당직원번호가 있으면 '있음', 없으면 '없음' 으로 표시

```sql
SELECT cus_no ,
       cus_name ,
       nvl(emp_no||'','없음')
  FROM customer;
```

* null 값 대체하는 함수
  * nvl(컬럼명,null 대체할 데이터)
    * 대체할 데이터에 원래자료형과 맞는지 비교해야됩니다.
    * 대체할 데이터가 원래 정수형이 였으면 컬럼명에 ||' ' 문자데이터를 붙여서 자료형이 문자처럼 보이게 바꾸고 사용하면됩니다.
* null 값 대체 함수 2
  * nvl2(컬럼명,null이 아닐때 대체 데이터, null값 대체 데이터)
    * 두개 다 바꾸기에 이건 자료형 상관 안해도 됩니다.



>####  employee 테이블에서 직원번호, 직원명 , 직급 , 성별를 출력하면?

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       CASE substr(jumin_num,7,1) 
           WHEN '1' 
           THEN '남' 
           WHEN'3' 
           THEN '남' 
           WHEN '2' 
           THEN '여' 
           WHEN '4' 
           THEN '여' endfrom employee;
```

* case 컬럼명(컬럼을 안고 있는 함수도 가능합니다.) when 1 then'남'
  * case는 조건을 얘기하고 when 어디를 then 어떤걸로 바꿀지 설정합니다 
  * 마지막에는 end를 통해 끝을 냅니다.
  * case 문도 자료형 끼리 확인을 해야합니다.



>  다른 방식의 case 문

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       CASE 
           WHEN substr(jumin_num,7,1) ='1' 
           THEN '남' 
           WHEN substr(jumin_num,7,1) ='3' 
           THEN '남' 
           ELSE '여' endfrom employee;
```

*      when substr(jumin_num,7,1) ='1' then '남' 
       * = 부분에 등호를 통해  **부등호** 관계를 통해서도 가능합니다. >=    <=



> **else** 를 통한 case 문

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       CASE substr(jumin_num,7,1) 
           WHEN '1' 
           THEN '남' 
           WHEN'3' 
           THEN '남' 
           ELSE '여' endfrom employee;
```

* if / else 처럼 case 문 나머지를 when then으로만 안하고 else 로 나머지를 지정 할 수 있습니다.



> **decode** 문

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       decode( substr(jumin_num,7,1) ,'1','남' ,'3','남' ,'여')
  FROM employee;
```

* decode (조건문),데이터1, '데이터 1과 같다면 출력 데이터', '  데이터2  ' , '  데이터2와 같다면 출력데이터  ' , '그외 출력 데이터'
  * , 컴마로만 표현되기 때문에 데이터와 같다라면 표현 밖에 되지않습니다
  * case 문을 보통 많이 사용합니다.

> #### employee 테이블에서 직원번호 , 직원명 , 직급 , 출생년도를 출생해주세요

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       CASE 
           WHEN substr(jumin_num,7,1)='1' 
           THEN'19' 
           WHEN substr(jumin_num,7,1)='2'
           THEN '19' 
           ELSE '20' 
       END || substr(jumin_num,1,2)
  FROM employee;
```



> #### decode 문

```sql
SELECT emp_no ,
       emp_name ,
       jikup ,
       decode(substr(jumin_num,7,1) ,'1','19'||substr(jumin_num,1,2) ,'2','19'||substr(jumin_num,1,2) ,'3','20'||substr(jumin_num,1,2)) 
  FROM employee;
```



> #### employee 테이블에서 직원번호 , 직원명 , 직급 ,출생년대(4자리) 출력

```
SELECT emp_no ,
       emp_name ,
       jikup ,
       CASE 
           WHEN substr(jumin_num,7,1)='1' 
           THEN'19' 
           WHEN substr(jumin_num,7,1)='2'
           THEN '19' 
           ELSE '20' 
       END || substr(jumin_num,1,2)
  FROM employee;
```



> #### employee 테이블에서 나이순으로 출력하면? 연장자가 먼저 나오도록.

```sql
select * from employee order by jumin_num desc;
```

* 위에 수식은 00년생이 오면 제일 연장자로 출력되기에 틀린 구문

```sql
SELECT * 
  FROM employee 
ORDER BY 
       CASE 
           WHEN substr(jumin_num,7,1) ='1' 
           THEN '19' 
           WHEN substr(jumin_num,7,1) ='2' 
           THEN '19' 
           ELSE '20' 
       END || substr(jumin_num,1,6) ASC
```

* 정렬기준에도 case 문을 사용할수있습니다
  * 숫자문자 는 문자열 안에 9부터 desc일경우 9부터 나열하여서 이렇게 정렬합니다.
  * order by 뒤에 case 구문이 나오면서 정렬에 기준이 된다.



> #### employee 테이블에서 직급 순서대로 정렬하여 모든 컬럼을 보이면?

```sql
SELECT * 
  FROM employee 
ORDER BY 
       CASE jikup 
           WHEN '사장' 
           THEN 1 
           WHEN '부장' 
           THEN 2 
           WHEN '과장' 
           THEN 3 
           WHEN '대리' 
           THEN 4 
           WHEN '주임' 
           THEN 5 
           ELSE 6 
       END ASC
```

* 직급을 수로 표현해서 나열합니다.
  * order by 뒤에 when 구문 적습니다.
  * 직급을 수 로 나눌땐 문자 말고 숫자로 하는게 좋습니다 10의 자릿수가 되면 구분이 안됩니다.

```sql
SELECT * 
  FROM employee 
ORDER BY decode(jikup,'사장',1,'부장',2,'과장',3,'대리',4,'주임',5,6)
```





> #### employee 테이블에서 직원번호 , 직원명 , 입사일(년 - 월 - 일) 검색하면?

```sql
SELECT emp_no ,
       emp_name ,
       to_char(hire_date,'yyyy-mm-dd') 
  FROM employee;
```

* to_char 지정한 날짜 또는 숫자를 원하는 출력 문자패턴으로 바꾸는 변환함수

```sql
SELECT emp_no ,
       emp_name ,
       to_char(hire_date,'yyyy-mm-dd(DAY) AM HH:MI:SS') 
  FROM employee;
```

* to_char(날짜 또는 숫자 , '출력 문자패턴')

* YYYY  : 년도 4자리
* MM    : 월 2자리
* DD      : 일 2자리
* AM HH : 오전 | 오후 1~12시 사이의 시간 표현
* HH 24   : 0~23 사이의 시간 표현
* MI : 0~ 59 사이의 분
* SS : MI : 0~ 59 사이의 초
* DAY : 영문 요일 풀네임
* DY    : 영문 요일 약어
* Q : 1~4 분기 표현
* 요일 AM / PM시간 분 초 출력
* 한글로 변환 - >     ,to_char(hire_date,'yyyy-mm-dd(DAY) AM HH:MI:SS','NLS_DATE_LANGUAGE = Korean') 
* day 대신 dy 로 입력하면 요일이 짧게 표현됩니다.
* am -> hh24 하면 21시  이런식으로 표현 가능



> #### employee 테이블에서 직원번호 , 직원명 , 입사일(년 - 월 - 일 분기 시 분 초) 검색하면

```sql
SELECT emp_no ,
       emp_name ,
       to_char(hire_date,'yyyy-mm-dd(DAY) Q AM HH:MI:SS','NLS_DATE_LANGUAGE = Korean') 
  FROM employee;
```

* 분기는 Q 입력

```sql
SELECT emp_no 직원번호 ,
       emp_name 직원명 ,
       to_char(hire_date,'YYYY')||'년' ||to_char(hire_date,'MM')||'월' ||to_char(hire_date,'DD')||'일' ||to_char(hire_date,'(DY)','NLS_DATE_LANGUAGE = Korean') ||to_char(hire_date,'Q')||'분기' ||to_char(hire_date,'AM HH')||'시' ||to_char(hire_date,'MI')||'분' ||to_char(hire_date,'SS')||'초' 
  FROM employee;

SELECT emp_no 직원번호 ,
       emp_name 직원명 ,
       to_char( hire_date ,'yyyy"년"mm"월"dd"월"(DAY) Q"분기"  HH"시":MI"분":SS"초"' ,'NLS_DATE_LANGUAGE = Korean') 입사일 
  FROM employee;
```

* ||TO+CHAR 사용하여 나눠 사용하거나
* 시분초에 더블 쿼터"" 사용하면 한번에 적을수 있다.



> #### employee 테이블에서 직원번호 , 직원명 , 나이 검색하면?

```sql
SELECT emp_no ,
       emp_name , 
       to_number(to_char(sysdate,'yyyy')) -to_number
       ( 
           CASE substr(jumin_num,7,1) 
               WHEN '1' 
               THEN '19' 
               WHEN '2' 
               THEN '19' 
               ELSE '20' 
           END || substr(jumin_num,1,2)
       )
       +1 || '세' 
  FROM employee;
```

* to_number : 숫자로 바꿔주는 함수 -> 숫자 문자만 변환할수 있습니다.



> #### employee 테이블에서 직원번호, 직원명 , 근무년차를 검색해서 출력

```sql
SELECT emp_no "직원번호" ,
       emp_name "직원명" ,
       ceil((sysdate-hire_date)/365)||'년차' ,
       오늘 날짜에서 입사일 날짜까지의 차이를 일수로 구하고 365로 나눈후 소수 첫째 자리에서 무조건 올림 => 근무년차from employee;
```

* oracle 에서는 날짜 데이터 - 날짜데이터 하면 차이를 일수를 리턴합니다
  * **ceil**   : 소수 첫째 자리 반올림 해주는 수학 함수
  * **floor** : 소수 첫째 자리에서 무조건 내림하는 수학 함수



> #### employee 테이블에서 직원번호, 직원명 , 연령대를 검색해서 출력

```sql
SELECT floor( to_number(to_char(sysdate,'yyyy')*0.1) - to_number
       (
           CASE substr(jumin_num,7,1) 
               WHEN '1' 
               THEN '19' 
               WHEN '2' 
               THEN '19' 
               WHEN '3' 
               THEN '20' 
           END||substr(jumin_num,1,2)*0.1
       )
       )||'0' "나이" 
  FROM employee  
```



> #### employee 테이블에서 직원번호, 직원명 , 100일잔치날짜 를 검색해서 출력

```sql
SELECT emp_no ,
       emp_name ,
       to_char( to_date
       ( 
           CASE 
               WHEN substr(jumin_num,7,1) IN ('1','2') 
               THEN '19' 
               ELSE '20' 
           END ||substr(jumin_num,1,6) ,'YYYYMMDD'
       ) 
       +100,'YYYY-MM-DD') 
  FROM employee;
```

* to_date(날짜문자)
  * 오라클은 날짜자료형에 정수를 더하거나 빼면 일수에 더해지거나 빼진 일수가 리턴이됩니다.
  * **날짜 + 날짜 return x 에러가 나옵니다**
    * 날짜 데이터 + 정수 -> 날짜 + 일수 가 됩니다.
    * 날짜 - 정수 : 날짜에 정수만큼의 일수를 뺀 날짜를 리턴
    * 날짜1 - 날짜2 : 날짜 1 과 날짜 2까지의 차이를 일수로 리턴



> #### 개강일이 2021년5월12일 이고 종강일이 2021년 11월 10일 이다.
>
> 며칠동안 학원 생활을 하나?

```sql
SELECT to_date('20211110','yyyymmdd') - to_date('20210512','yyyymmdd') 
  FROM dual;
```

* **dual** : 실존하지 않은 더미 테이블 입니다.



> ### employee 테이블에서 직원번호 , 직원명 , 현재나이 , 입사일 당시나이 를 검색해서 출력해주세요

```sql
SELECT emp_no "직원번호" ,
       emp_name "직원명" , 
       to_number(to_char(sysdate,'yyyy')) - to_number
       (
           CASE substr(jumin_num,7,1) 
               WHEN '3' 
               THEN '20'
               ELSE '19' 
           END || substr(jumin_num,1,2)
       )
       +1 ||'세' AS "현재나이" ,
       to_number(to_char(hire_date,'yyyy')) - to_number
       ( 
           CASE substr(jumin_num,7,1) 
               WHEN '3' 
               THEN'20' 
               ELSE '19' 
           END || substr(jumin_num,1,2)
       )
       +1||'세' "입사일당시 나이" 
  FROM employee;
```



> ### employee 테이블에서 직원번호, 주민번호 , 다가올생일  날(xxxx-xx-xx) , 생일까지 남은일수를 계산하면

```sql
SELECT emp_no "직원번호" ,
       emp_name "직원명" ,
       jumin_num "주민번호" ,
       CASE 
           WHEN to_date( to_char(sysdate,'yyyy')||substr(jumin_num,3,4),'yyyymmdd') - sysdate > =0 
           THEN to_char(to_date( to_char(sysdate,'yyyy')||substr(jumin_num,3,4) ,'yyyymmdd'),'yyyy"년"mm"월"dd"일"') 
           ELSE to_char(to_date( to_number(to_char(sysdate,'yyyy'))+1||substr(jumin_num,3,4) ,'yyyymmdd') ,'yyyy"년"mm"월"dd"일') 
       END "다가올생일날" ,
       CASE 
           WHEN to_date( to_char(sysdate,'yyyy')||substr(jumin_num,3,4),'yyyymmdd') - sysdate > =0 
           THEN floor(to_date( to_char(sysdate,'yyyy')||substr(jumin_num,3,4),'yyyymmdd') - sysdate) 
           ELSE floor(to_date( to_char(sysdate,'yyyy')+1||substr(jumin_num,3,4),'yyyymmdd') - sysdate) 
       END "남은생일" 
  FROM employee 
ORDER BY "남은생일" desc;  
```

* 생일까지 남은 일수 계산 방법
  * 만약 올해생일 - 지금날짜 값이 양수이면 생일이 지나지않았으므로 올해생일날짜 - 지금날짜
  * 만약 올해생일 - 지금날짜 값이 음수이면 생일이 지낫으므로 내년생일 - 지금날짜



> #### employee 테이블에서 직원번호 , 직원명 , 직급 ,연봉(xxx,xxx,xxx)만원 를 검색하면

```sql
SELECT emp_no "직원번호" ,
       emp_name "직원명" ,
       jikup "직급" ,
       to_char(salary,'999,999,999') "연봉" 
  FROM employee;
```

* 온점 으로 숫자 구분줄수 있는방법

  * 0아니면 9만 지정할수 있습니다.
  * to_char(salary,'999,999,999') ->salary 컬럼안의 숫자를 3자리마다 끊어서' , '를  삽입해서 문자로 리턴합니다. 만약 각 9자리에 대응하는 숫자 없으면 화면에 출력이 안된다.
  * * to_char(salary,'099,999,999') ->salary 컬럼안의 숫자를 3자리마다 끊어서' , '를  삽입해서 문자로 리턴합니다. 만약 맨앞에 0이 있으면 그자리가 비면 0 이 대체된다. 따라서 나머지도 9이어도 다 0으로 대체된다.	 **숫자범위가 길때 사용**

  ```sql
  SELECT emp_no "직원번호" ,
         emp_name "직원명" ,
         jikup "직급" ,
         to_char(salary,'000,000,000') ||'만원' "연봉" 
    FROM employee; 
  --출력값
  --000,005,000
  --000,003,000  
  ```

* 0를 사용하면 빈자리에 숫자를 매핑시켜줍니다.



> #### 테이블에서 수요일에 태어난 직원을 검색하라

```sql
SELECT * 
  FROM employee 
 WHERE to_char(to_date(decode(substr(jumin_num,7,1),'1','19','2','19','20')||substr(jumin_num,1,6),'yyyymmdd'),'day','nls_date_language=korean')='수요일'
```

* date에 day 를 d만적어 부등호 통해서 요일 비교 할 수 있다 d=1로 검색됩니다.



> #### employee 테이블에서 70년대생 남자 직원을 검색하라

```sql
SELECT * 
  FROM employee 
 WHERE substr(jumin_num,1,1)=7 
       AND substr(jumin_num,7,1)=1
```



> ### employee 테이블에서 1960년대 , 1970년대 출생자중 남자만 검색하라

```sql
SELECT * 
  FROM employee 
 WHERE (
           substr(jumin_num,1,1)='7' 
           OR substr(jumin_num,7,1)='6' 
       ) 
       AND 
       ( 
           substr(jumin_num,7,1)='1' 
           OR substr(jumin_num,7,1)='3' 
       ) 
```

* and 와 or 에 우선순위를 조심해야한다

  * **조심!**  괄호를 안묶으면 and 부분 먼저 계산이 된다

  

> #### employee 테이블에서 직급이 과장이 아닌 직원을 검색하면

```sql
SELECT * 
  FROM employee 
 WHERE jikup !='과장' 
SELECT * 
  FROM employee 
 WHERE jikup <>'과장'  
```

* **!=** 말고 다르다라는 표시를 oracle에선 **<>**를 사용도 가능하다

> #### employee 테이블에서 부서번호가 10번이고 직급이 과장인 직원을 검색하면?

```sql
select * from employee wheredep_no='10' and jikup = '과장'
```



> #### employee 테이블에서  직급이 과장또는 부장인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE jikup = '과장' 
       OR jikup = '부장' 
SELECT * 
  FROM employee 
 WHERE jikup IN('과장','부장');  

SELECT * 
  FROM employee 
 WHERE jikup = ANY('과장','부장');
```

* 동일한 컬럼이면 in을 통해 중복을 제거 할 수 있다.  **컬럼명 in (비교데이터,비교데이터)**

  * **in**    =   or 에 의미를 가지고 있다.
    * in 은 좌우에 and가 있어도 먼저 계산됩니다.
  * **any** =  in 과 같은 의미이다.

  

> #### employee 테이블에서 10번, 20번 부서 중에 직급이 과장인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE( 
           dep_no = '10' 
           OR dep_no = '20' 
       ) 
       AND jikup = '과장' 
SELECT * 
  FROM employee 
 WHERE dep_no IN(10,20) 
       AND jikup='과장';
```

* **in** 은 좌우에 and가 있어도 먼저 계산됩니다.



> #### customer 테이블에서 담당직원이 없는 고객을 검색하면?

```sql
select * from customer 
where emp_no is null
```



> #### customer 테이블에서 담당직원이 있는 고객을 검색하면?

```sql
select * from customer
where emp_no is not null
```



> #### customer 테이블에서 담당직원 번호가 9번이 아닌 고객을 검색하면?

```sql
SELECT * 
  FROM customer 
 WHERE emp_no !=9 
       AND emp_no IS NULL 
SELECT * 
  FROM customer 
 WHERE emp_no !=9 
       OR emp_no IS NULL
```

* null값은 계산 되지않습니다.
  * or 통해 null값도 같이 출력
  * **주의** null은 is null 또는 is not null 연산자에 의해서만 검색된다.



> #### employee 테이블에서 연봉이 3000만원 ~ 4000만원 사이인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE salary >= 3000 
       AND salary <=4000 
SELECT * 
  FROM employee 
 WHERE salary BETWEEN 3000 AND 4000
```

* 컬럼명 between 비교데이터 and or
* 등호식 표현할때 반복을 줄이기 위해서 between도 사용 가능하다 between: ~이상 ~이하



> #### employee 테이블에서 연봉이 3000만원 이상~4000만원 미만 사이인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE salary >= 3000 
       AND salary <4000 
SELECT * 
  FROM employee 
 WHERE salary BETWEEN 3000 AND 4000 
       AND salary!=4000
```

* 이런식으로 between 가능 하지만 보기 불편하다.



> #### employee 테이블에서 연봉을 5% 인상할 경우 3000 이상인 직원을 검색하면?

```sql
select * from employeewhere salary*1.05>=3000
```



> #### employee 테이블에서 입사일이 '1995-1-1' 이상인 직원을 검색하면?

```sql
select * from employeewhere to_date('1995-01-01','yyyy-mm-dd') <=hire_date
```



> #### employee 테이블에서 입사일이 1990년 1999년 사이인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE dep_no IN(10,30) 
       AND salary<3000 
       AND hire_date < to_date('1996-01-01','yyyy-mm-dd');  

SELECT * 
  FROM employee 
 WHERE dep_no =ANY(10,30) 
       AND salary<3000 
       AND hire_date < to_date('1996-01-01','yyyy-mm-dd');  
```

* hire_date 에서 년수 뽑아서 합니다.



> #### employee 테이블에서 부서번호가 10번 또는 30번인 직원 중에 연봉이 3000미만이고 입사일이 '1996-01-01' 미만 직원을 검색하면?

```sql
select * from employeewhere dep_no in(10,30)and salary<3000and hire_date < to_date('1996-01-01','yyyy-mm-dd');  select * from employeewhere dep_no =any(10,30)and salary<3000and hire_date < to_date('1996-01-01','yyyy-mm-dd');  
```



> #### employee 테이블에서 성이 김씨인 직원을 검색하면?

```sql
select * from employeewhere substr(emp_name,1,1) ='김'select * from employee where emp_name like '김%'
```

* like 연산자

  * where 컬럼명 like '패턴문자열%'
    * 컬럼명안의 데이터가 패턴문자열을 갖고 있으면 그 행을 검색하라
  * '김%'
    * 김이 첫글자이고 두번째는 무엇이와도 좋고 길이에 제한없이 문자패턴 행을 리턴
    * 문자 패턴 안의 %는 무엇이 와도 좋고 길이에 제한없음의 의미이다.
  * '%김'
    * 김이 마지막글자인 사람의 행을 리턴

* lie 연산자의 반대

  * where 컬럼명 not like '패텬문자열%'

  

> #### employee 테이블에서 성이 황씨인 직원을 검색하면?

```sql
--성이 황보 씨인 사람은 제외하고 보여야됩니다.
SELECT * 
  FROM employeewhere substr(emp_name,1,1) ='황'  
SELECT * 
  FROM employee 
 WHERE substr(emp_name,1,1) ='황%' 
       AND substr(emp_name,1,2)!='황보';

SELECT * 
  FROM employeewhere substr(emp_name,1,1) ='황%' 
       AND substr(emp_name,1,2)<>'황보';

SELECT * 
  FROM employeewhere substr(emp_name,1,1) ='황' 
       AND emp_name NOT LIKE '황보%'
```

* 성이 한글자가 아닌 사람도 있으니 성이 여러 글자인 사람은 소거해야된다



> #### employee 테이블에서 이름이 2자인 직원을 검색하면?

```sql
select * from employee where length(emp_name) = 2
```

* length(컬럼명) : 컬럼명 안의 문자 데이터 길이를 0이상의 정수로  리턴



> #### employee 테이블에서 이름이 김으로 끝나는 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE emp_name LIKE '%김'
SELECT * 
  FROM employee 
 WHERE substr(emp_name,length(emp_name),1) = '김'
```



> #### employee 테이블에서 성이 김씨이고 3글자인 직원을 검색하면?

```sql
SELECT * 
  FROM employee 
 WHERE substr(emp_name,1,1) = '김' 
       AND length(emp_name)=3select * 
  FROM employee 
 WHERE emp_name LIKE '김%' 
       AND length(emp_name)=3select * 
  FROM employee 
 WHERE emp_name LIKE '김__' 
       AND length(emp_name)=3
```

* _ : 는밀고 뒤에칸을 비교하는거



> #### employee 테이블에서 이름에 김이란 문자를 가진 직원을 검색하면?

```sql
select * from employee
where emp_name like '%김%'
```

* % 양쪽에 붙일수도 있다.

> #### employee 테이블에서 성이 김씨가 아닌 직원을 검색하면?

```sql
select * from employee
where emp_name not like '김%'
```



> #### employee 테이블에서 이름 중간에만 김이 들어간 직원을 검색하면>

```sql
SELECT * 
  FROM employee 
 WHERE emp_name NOT LIKE '김%' 
       AND emp_name NOT LIKE '%김' 
       AND emp_name LIKE '%김%'  
```



> #### employee 테이블에서 여자 직원을 검색하라.

```sql
SELECT * 
  FROM employeewhere substr(jumin_num,7,1) =2 orsubstr(jumin_num,7,1) =4select * 
  FROM employeewhere substr(jumin_num7,1) IN (2,4)
SELECT * 
  FROM employee wherejumin_num LIKE '______2%' 
       OR jumin_num LIKE '______4%'
```

* 무엇이와도 좋아 단 _ 는 한자로 칩니다.



> #### 만약 주민번호 중간에 - 가 있다면 아래 처럼해도 된다.

```sql
select * from employee where	jumin_num like'%_2%' or jumin_num like '%_4%';
```



> #### employee 테이블에서 60년대 70년대 출생자중 남자만 검색

```sql
SELECT * 
  FROM employee 
 WHERE (
           substr(jumin_num,1,1)=6 
           OR substr(jumin_num,1,1)=7
       )
       AND
       (
           substr(jumin_num,7,1)=1 
           OR substr(jumin_num,7,1)=3
       )
SELECT * 
  FROM employee wherejumin_num LIKE '6_____1%' 
       OR jumin_num LIKE '7_____1%'
SELECT * 
  FROM employee wheresubstr(jumin_num,1,1) IN ('6','7') 
       AND substr(jumin_num,7,1) ='1'
```

---

# 