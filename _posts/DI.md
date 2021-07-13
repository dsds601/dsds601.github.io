# DI

* ### 의존성 주입(Dependency Injection)


* ### 주입이란?

  * xml에 bean을 정의할때 해당 bean객체가 생성이될때 각각변수에 어떤값을 넣을지 정하는 개념이자 장점이다.
  * Bean 객체를 생성할 때 Bean 객체가 관리할 값이나 객체를 주입하는것을 의미한다.
  * Bean객체 생성 후 Bean객체가 가질 기본 값을 자바 코드로 설정하는 것이 아닌 Bean을 정의하는 xml 코드에서 정의하는 개념이다.



* 자바 생성자를 통한 주입

  * 자바에서 default 생성자외 생성자를 만들때 매개변수를 받아서 생성한다

  * xml을 통한 주입
  * Bean을 정의할때 contructor-arg 태그를 이용하여 주입하게 되면 생성자를 통해 주입할 수 있다.

  

  | value | 기본자료형 값과 문자열 값을 설정한다.           |
  | ----- | ----------------------------------------------- |
  | ref   | 객체를 설정한다                                 |
  | type  | 저장할 값의 타입을 설정한다.                    |
  | index | 지정된 값을 주입할 생성자의 매개변수 인덱스번호 |

  

  ```java
  //default 생성자
  TestBean t1 = new TestBean();
  		t1.printDate();
  //매개 변수를 받아 생성	
  		TestBean t2 = new TestBean(100);
  		t2.printDate();
  ```

  * 스프링에서 default외 생성자 만드는법
  * **contructor-arg>태그안에 value 값을 넣는다 이것을 주입이라고 합니다**

  ```xml
  <!-- default 생성자-->
  	<bean  id="obj1" class="kr.co.beans.TestBean" lazy-init="true"/>
  <!-- 매개변수를받은 생성자-->	
  	<bean id="obj2" class="kr.co.beans.TestBean" lazy-init="true">
  	<constructor-arg value="100"/>
  	</bean>
  
  <!-- 객체를 만들때는 동일-->
  TestBean obj2 = ctx.getBean("obj2",TestBean.class);
  		obj2.printDate();
  		ctx.close();
  ```

  

  * 스프링에서는 int 타입보다 double 자료형을 우선시함

  ```java
  System.out.println("---------------------------------------");
  //t3생성자 double 자료형을 받아서 생성
  		TestBean t3 = new TestBean(11.41);
  		t3.printDate();
  //obj2 는 value를 하나 받는 생성자인데도 xml에 int 자료형이여도 double 영향을 받아서 결과 값이 달라짐		
  TestBean obj2 = ctx.getBean("obj2",TestBean.class);
  		obj2.printDate();
  		ctx.close();		
  ```

  * bean.xml에 실수 값을 받는 bean을 만들고 실행시

  ```xml
  	<bean id="obj3" class="kr.co.beans.TestBean" lazy-init="true">
  	<constructor-arg value="11.11"/>
  	</bean>
  ```

  * 자바 실행

  ```java
  TestBean obj2 = ctx.getBean("obj2", TestBean.class);
  		obj2.printDate();
  		System.out.println("---------------------------------------");
  
  		TestBean obj3 = ctx.getBean("obj3", TestBean.class);
  		obj3.printDate();
  		ctx.close();
  ```

  * obj2에는 값으로 100이 아니라 실수형으로 변형되어 100.000이 나오고
  * obj3에는 11.110000이 나옵니다.
  * **String 타입의 객체로 생성자 생성 후 실행**

  ```xm
  	<bean id="obj4" class="kr.co.beans.TestBean" lazy-init="true">
  	<constructor-arg value="문자열"/>
  	</bean>
  	
  	value를 넣은 bean.xml로 만든 다른 객체들도 data3 자료형 타입 위치에 자료들이
  	가 있는걸 확인 할 수있다.
  	TestBean의 생성자 : String 변수 1개
  date1 : 0
  date2 : 0.000000
  date3 : 11.1
  ```

  * spring에서는 String  - double - int 순서대로 값을 주입한다.
  * 타입별로 값을 지정해주고 싶을 경우 xml에 타입을 따로 지정해야 한다.

  ```xml
  <bean id="obj2" class="kr.co.beans.TestBean" lazy-init="true">
  	<constructor-arg value="100" type="int"/>
  	</bean>
  	
  	<bean id="obj3" class="kr.co.beans.TestBean" lazy-init="true">
  	<constructor-arg value="11.11" type="double"/>
  	</bean>
  ```

  * type ="int" 자료형을 따로 지정하여야 원하는 자료형에 들어간다 String을 최우선 하니 따로 안적어도 무방하다.
    
  * 여러개 생성자를 입력하는 경우
  * contructor arg를 여러번 자료형에 맞게 기입한다. String은 클래스명 모두 작성
  
  ```xml
  <constructor-arg value="200" type="int"/>
  	<constructor-arg value="22.22" type="double"/>
  	<constructor-arg value="안녕" type='java.lang.String'/>
  ```



* 자바코드에서 생성자를 작성할때 생성자 순서대로 입력해야 오류가 나지않는다.

  * spring 에서는 xml에 자료형이 섞여도 주입이 가능한 자료형들이 모여 있다면 그 자료들로 매핑을 시킨다. 그래도 순서대로 기입하길!

  ```xml
  	<bean id="obj6" class="kr.co.beans.TestBean" lazy-init="true">
  		<constructor-arg value="안녕" type='java.lang.String' />
  		<constructor-arg value="22.22" type="double" />
  		<constructor-arg value="200" type="int" />
  	</bean>
  	
  	<!-- 출력값-->
  	TestBean의 생성자 : 변수3개
  date1 : 200
  date2 : 22.220000
  date3 : 안녕
  ```

  * spring에서는 같은 자료형이 여러개가 나오는 생성자에 index 순서를 정해줄수있다.

  ```xml
  	<bean id="obj7" class="kr.co.beans.TestBean" lazy-init="true">
  		<constructor-arg value="44.44" type='double' index="1" />
  		<constructor-arg value="22" type="int" index="0" />
  		<constructor-arg value="하이" index="2"/>
  	</bean>
  ```

  



* ### 객체를 주입하는 법

  * 객체를 주입할땐 contuctort arg 태그 사이에 기입한다.
  * 객체가 여러개일경우 여러 contructor arg 태그 사이에 기입한다

```xml
<bean id ="obj8" class="kr.co.beans.TestBean2" lazy-init="true">
	<constructor-arg>
	<bean class="kr.co.beans.DataBean"/>
	</constructor-arg>
	
	<constructor-arg>
	<bean class="kr.co.beans.DataBean"/>
	</constructor-arg>
	</bean>
```



* xml 에 정의되어있는 객체를 주입할 경우

```xml
	<!-- 정의 되어있는 bean-->
	<bean id ="data_bean" class="kr.co.beans.DataBean" scope="prototype"/>
	
	<bean id="obj9" class="kr.co.beans.TestBean2" lazy-init="true">
	<!--TestBean2(DataBean , DataBean) 동일-->
	<constructor-arg ref="data_bean"/>
	<constructor-arg ref="data_bean"/>
	</bean>
	
```





* ### Setter 메서드를 통한 주입

  * Bean을 정의할 때 Bean 객체가 가지고 있을 기본 값을 생성자가 아닌 Setter 메서드를 통해 주입할 수 있다.



| name  | 데이터를 주입할 프로퍼티의 이름                 |
| ----- | ----------------------------------------------- |
| value | 기본 자료형 및 문자열을 주입할 때 사용하는 속성 |
| ref   | 객체의 주소 값을 주입할 때 사용하는 속성        |

![](https://github.com/dsds601/java/blob/main/img/set%20%EC%A3%BC%EC%9E%85%EC%9B%90%EB%A6%AC.png?raw=true)

* property 태그를 이용하여 name="data1"를 작성하면  앞에set을 붙여 setdata1를 찾아 호출하고
  value값을 data1에 넣는다.

* class 파일에 get / set 메서드를 지정한다. 

```
private int data1;

	public int getData1() {
		return data1;
	}

	public void setData1(int data1) {
		this.data1 = data1;
	}

}
```



* Xml.

```
<bean id='t1' class='kr.co.beans.TestBean'>
	<property name="data1" value="100"/>
	</bean> 
```

* java

```
ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("kr/co/config/beans.xml");
		TestBean t1 = ctx.getBean("t1",TestBean.class);
		System.out.println(t1.getData1());
```



* 객체와 정의 되어있는 객체를 주입할때 xml

```
<bean id='t1' class='kr.co.beans.TestBean'>
	<property name="data1" value="100"/>
	<property name="data2" value="11.11"/>
	<property name="data3" value="true"/>
	<property name="data4" value="안녕"/>
	<property name="data5">
		<bean class="kr.co.beans.DataBean"/>
	</property>
	<property name="data6" ref="data_bean"/>
	</bean> 
	
	<bean id ="data_bean" class="kr.co.beans.DataBean"/>  <!-- data6 입니다. -->
```

* xml에 정의 되어있는 객체를 주입할때는 ref 를 이용하여 주입한다.
