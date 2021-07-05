### 아래 고객 요구사항에 맞는 테이블을 만들면?

![](D:\zzz\수업자료\dept.employee.cutomer.salary 테이블 구조001.jpg)

오라클에서 모든 문자는 싱글쿼터 '' 로 감쌉니다.

; 는 객체 한개 생성할때는 없어도 생성이 됩니다.

```sql
create table dept(
dep_no number(3) primary key
,dep_name varchar2(20) not null unique
,loc varchar2(20) not null);
```

드래그 하면 주석 처리 되어도 실행이 됩니다.

```sql
insert into dept(dep_no,dep_name,loc)values(10,'총무부','서울');
```

컬럼명 순서 바꾸어서 작성 가능 단 values랑 매핑이되게 맞게 작성하셔야 됩니다.

```sql
insert into dept values(10,'총무부','서울');
```

컬럼명 생략 가능하나 create시 만드는 컬럼명 순서랑 동일해야 됩니다.

default시 sysdate 현재 날짜시간 설정.

table에 행 삭제 : 삭제 원하는 테이블 탭에서 더블클릭 -> 상단 탭 데이터 -> 삭제 원하는행 '-' 클릭

테이블내 모든 항 삭제 : 

 ```sql
delete from employee; 
 ```

```
drop table employee; 삭제
```





insert  **into**  dept(컬럼명) values(내용);   into 조심



##### where 절

* select * from dept **where dep_no** =10;
  * dep_no 10 번인  자료 보여줘
  * **주의** 모든 행 이란 말이 나오면 **WHER** 절이 없습니다.



---

* 부서관리 테이블
  * 관리하고 싶은 정보는 부서명 , 부서 위치 입니다.

```sql
create table dept(
 dep_no number(3) 
 ,dep_name varchar2(20) not null unique 
 ,loc varchar2(20) not null 
 , primary key(dep_no));
```

* 직원정보 관리 테이블
  * 관리하고 싶은 정보는 직원명 , 직급 , 입사일 , 소속부서명 , 연봉 , 주민번호 , 전화번호 , 연봉등급 , 직속 상관명 입니다.

```
sql
create table employee(
 emp_no number(3) 
 ,emp_name varchar2(20) not null 
 ,dep_no number(3) 
 ,jikup varchar2(20) not null 
 ,salary number(9) default 0 
 ,hire_date date default sysdate 
 ,jumin_num char(13) not null unique 
 ,phone varchar2(15) not null 
 ,mgr_emp_no number(3) 
 , primary key(emp_no) 
 , foreign key(dep_no) references dept(dep_no)
 , constraint employee_mgr_emp_no_fk foreign key(mgr_emp_no) references employee(emp_no));
```

* 고객 정보 관리 테이블
  * 관리하고 싶은 정보는 고객명 , 전화번호 , 주민번호 , 담당직원명 입니다.

```sql
create table customer( 
cus_no number(3) 
,cus_name varchar2(20) not null 
,tel_num varchar2(15) 
,jumin_num char(13) not null unique 
,emp_no number(3) 
,primary key(cus_no); 
,foreign key(emp_no) references employee(emp_no));
```

* 연봉등급 관리 테이블
  * 관리하고 싶은 정보는 연봉등급 , 등급별최소연봉 , 등급별최대연봉

```sql
create table salary_grade ( 
sal_grade_no number(2) 
,min_salary number(5) not null 
,max_salary number(5) not null 
,primary key (sal_grade_no) );
```

---
