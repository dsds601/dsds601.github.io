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



* ### 컬렉션 주입

  * Bean을 정의할 때 주입해야 하는 멤버가 컬렉션인 경우 컬렉션이 관리할 객체를 초기에 설정할 수 있다.
  * 여기에서는 List, Map, Set ,Property를 사용한다.

* #### list 형태에 뼈대

  1. bean으로 클래스를 불러오기
  2. property로 set메서드 정의할 메서드명 작성
  3. 컬렉션에 형태 list / arrayList 작성
  4. value 값에 안에 넣을 값 지정 스트링타입이 아니면 type으로 자료형 적으면 좋다

  ```xml
  	형태
  <bean id="t1" class="클래스 위치">
  <property name ="list1">  set메서드에 set제외한 메서드명
      <list>  -컬렉션 프레임 자료형
      <value type="">   컬렉션에 들어갈 자료형
          <bean class="객체 위치" />  / 객체가 컬렉션 프레임워크 자료형일 경우 bean으로 정의한다
          </value>
      </list>
      </property>
  </bean>
  ```

  

* 컬렉션을 관리하는 클래스

```java
public class TestBean {

	private List<String> list1;

	public List<String> getList1() {
		return list1;
	}

	public void setList1(List<String> list1) {
		this.list1 = list1;
	}
```



* bean을 관리하는 xml

```xml
<bean id="t1" class="kr.co.beans.TestBean">
	<!-- 제네릭이 String인 List -->
	<property name ="list1">
		<!-- list가 관리할 문자열을 list에 value 형태로 넣는다. -->
		<list>
			<value>문자열1</value>   
			<value>문자열2</value>
			<value>문자열3</value>
		</list>
	</property>
	</bean>
```

* 문자열을 관리하는 list멤버변수명이 list1이라 set/get으로 setList1이 나와서 xml property name="list1"으로 설정 하고 list 태그안에 list컬렉션이 관리할 자료를 value 태그 안에 작성한다. 
  list3개를 가지고있는 객체를 만든다.



* list 컬렉션을 출력하는 main 메서드

```java
ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("kr/co/config/beans.xml");
		TestBean t1 = ctx.getBean("t1",TestBean.class);
		List<String> list1 = t1.getList1();
		for(String str :list1) {
			System.out.printf("list1 : %s\n" ,str);
		}
		
		ctx.close();
	}
```

* Integer 타입을 자료로 갖는 list .xml

  ```xml
  <bean id="t1" class="kr.co.beans.TestBean">
  		<!-- 제네릭이 String인 List -->
  		<property name="list1">
  			<!-- list가 관리할 문자열을 list에 value 형태로 넣는다. -->
  			<list>
  				<value>문자열1</value>
  				<value>문자열2</value>
  				<value>문자열3</value>
  			</list>
  		</property>
  		<!-- 제네릭이 Integer인 List -->
  		<property name="list2">
  			<list>
  				<value type="int">100</value>
  				<value type="int">200</value>
  				<value type="int">300</value>
  			</list>
  		</property>
  
  	</bean>
  ```

  * 자료형이 스트링 형태가 아니라면 type을 지정해주는게 좋다

  

  * Integer형태를 가지고 있는 list 컬랙션 main 메서드

```java
List<Integer> list2 = t1.getList2();
		for(int value : list2) {
			System.out.printf("list2 : %s\n" ,value);
		}
		ctx.close();
	}
```



* 객체형태를 가지고 있는 list xml

```
<property name="list3">
			<list>
			<bean class="kr.co.beans.DataBean"/>
			<bean class="kr.co.beans.DataBean"/>
			</list>
		</property>
```

* 기존에 정의되어있는 객체를 list에 넣는 경우 ref를 이용

```xml
<property name="list3">
			<list>
			<ref bean="data_bena"/>   <-- ref에는 정의된 객체 ud를 넣는다-->
			<ref bean="data_bena"/>
			</list>
		</property>
```

* 정의된 객체에 scope가 prototype일 경우 객체를 부를때마다 객체가 생성되어 메모리 위치주소가 각각 달라진다.



* 객체 형태를 호출하는 main 메서드

```
List<DataBean> list3 = t1.getList3();
		for(DataBean obj : list3) {
			System.out.printf("list3 : %s\n", obj);
		}
		
```







* #### set 프레임워크

  * set프레임워크에 동일한 성질을 가지고있어서 동일한 값은 중복이 안된다.
  * id 가 똑같은 bean을 주입한 경우

  ```xml
  <property name="set3">
  			<set>
  				<bean class="kr.co.beans.DataBean" />
  				<bean class="kr.co.beans.DataBean" />
  				<ref bean="data_bean" />
  				<ref bean="data_bean" />
  				<ref bean="data_bean" />
  			</set>
  		</property>
  ```

  * id 가 동일한 bean 3개를 주입

  

  * set을 호출하는 main 메서드

  ```java
  Set<String> set1 = t1.getSet1();
  		Set<Integer> set2 = t1.getSet2();
  		Set<DataBean> set3 = t1.getSet3();
  		
  		for (String str : set1) {
  			System.out.printf("set1 : %s\n", str);
  		}
  		
  		
  		for (int value : set2) {
  			System.out.printf("set2 : %s\n", value);
  		}
  		
  		for(DataBean obj : set3) {
  			System.out.printf("set3 : %s\n", obj);
  ```

  * set은 중복을 허용하지않아서 id는 동일한 객체를 불러와서 1번만 호출이된다.
  * scope가 prototype이기에 객체는 매번 호출할때마다 생성이 되지만 **id를 통해 부르는경우는 동일한 객체가 나온다.**



* ### Map

  * 타입에 관계없이 매개체 하나의 정보를 관리할때 사용된다.

    

  * Map 컬렉션 xml

  * map에 key 와 value 형태로 list 나 set 을 그형태에 맞게 넣는다.

  ```xml
  <map>
  		<entry key ="a1" value="문자열"/>
  		<entry key ="a2" value="100" value-type="int"/>
  		<entry key ="a3">
  			<bean class="kr.co.beans.DataBean"/>
  		</entry>
  		<entry key="a4" value-ref="data_bean"/>
  		<entry key="a5">
  			<list>
  			<value>문자열1</value>
  			<value>문자열2</value>
  			</list>
  		</entry>
  		<entry key="a6">
  		<set>
  		<value type="int">10</value>
  		<value type="int">20</value>
  		</set>
  		</entry>
  	</map>
  	</property>
  ```

  * main 메서드

  ```java
  Map<String, Object> map1 = t1.getMap1();
  		String a1 = (String)map1.get("a1");
  		int a2 = (Integer)map1.get("a2");
  		DataBean a3 = (DataBean)map1.get("a3");
  		DataBean a4 = (DataBean)map1.get("a4");
  		List<String>a5 = (List<String>)map1.get("a5");
  		Set<Integer>a6 = (Set<Integer>)map1.get("a6");
  		
  		System.out.printf("a1 : %s\n ",a1);
  		System.out.printf("a2 : %s\n ",a2);
  		System.out.printf("a3 : %s\n ",a3);
  		System.out.printf("a4 : %s\n ",a4);
  ```

  

* ### propertie

  * 문자 값만 저장할수있습니다.

    

  * class 파일

  ```java
  private Properties prop1;
  ```

  

  * xml 파일

  ```xml
  <!-- property -->
  	<property name="prop1">
  		<props>
  		<prop key="p1">문자열1</prop>
  		<prop key="p2">문자열2</prop>
  		<prop key="p3">문자열3</prop>
  		</props>
  	</property>
  	</bean>
  ```

  

* main 메서드

  ```java
  Properties prop1 = t1.getProp1();
  		
  		String p1 = prop1.getProperty("p1");
  		String p2 = prop1.getProperty("p2");
  		String p3 = prop1.getProperty("p3");
  		
  		System.out.printf("p1 : %s\n",p1);
  		System.out.printf("p2 : %s\n",p2);
  		System.out.printf("p3 : %s\n",p3);
  ```





* ### 자동주입

  * Bean을 정의할 때 주입할 객체는 생성자를 통한 주입이나 setter를 통한 주입을 사용했다.
  * Spring에서는 **객체**를 주입할 때 자동으로 주입될 수 있도록 설정할 수 있다.
    * 자료형은 자동주입이 안되고 참조자료형 같은 객체타입은 가능하다!

  * 자동 주입은 이름, 타입 , 생성자르 통할 수 있으며 auto wire라는 용어로 부른다.



* #### 이름을 통한 주입

  * AutoWire에 속성을 넣어 실행한다.
  * byName : 빈 객체의 프로퍼티 이름과 정의되 빈의 이름이 같은 것을 찾아 자동으로 주입한다.
    * **기본자료형은 안됩니다!**

  ![](https://github.com/dsds601/java/blob/main/img/byname.png?raw=true)



```xml
<!-- 자동주입 -->
	<bean id="obj2" class="kr.co.beans.TestBean1" autowire="byName"/>
	
	
	<!-- 넣을 객체에 id와 클래스에 정의된  변수명과 맞춘다 -->
	<bean id="data1" class="kr.co.beans.DataBean1" />
	<bean id="data2" class="kr.co.beans.DataBean1"/>

class속
public class TestBean1 {

	private DataBean1 data1;
	private DataBean1 data2;
```

* 객체 클래스를 만든 xml에 autowire ="byname"를 맞춘다.
* 클래스 속에 들어갈 객체 변수명과 xml속 bean id와 같은걸 매핑 시켜준다.





* ### 타입을 통한 주입

  * byType : 빈 객체의 프로퍼티 타입과 정의된 빈의 타입이 일치할 경우 주입된다.
  * 이 때, **동일 타입의 빈이 두개 이상 정의되어 있으면 오류**가 발생한다.
    * 어떤 타입을 지정할지 몰라서 오류가 발생!!

  ![](https://github.com/dsds601/java/blob/main/img/bytype.png?raw=true)

  

* class

```java
TestBean2 obj3 = ctx.getBean("obj3",TestBean2.class);
		System.out.printf("obj3.data1 : %s\n ",obj3.getData1());
		System.out.printf("obj3.data2 : %s\n ",obj3.getData2());
```

* xml

```xml
<!-- 자동타입 -->
	<bean id="obj3" class="kr.co.beans.TestBean2" autowire="byType"/>
	<!-- byType은 자료형만 맞으면  자동주입이라 id가 필요없다.-->
	<bean class="kr.co.beans.DataBean2"/>
```



* xml 에러

```xml
<!-- 동일한 자료형이 2개이상이면 에러가 난다.-->
	<bean class="kr.co.beans.DataBean2"/>
	<bean class="kr.co.beans.DataBean2"/> 
```





* ### 생성자를 통한 주입

  * constructor : 생성자의 매개 변수 타입과 정의된 빈의 타입이 일치할 경우 주입된다.
  * 이 때,동일 타입의 빈이 두 개 이상 정의되어 있으면 오류가 발생한다.
    * **autowire으로 주입이됩니다.****

  ![](https://github.com/dsds601/java/blob/main/img/contructor%20%EC%A3%BC%EC%9E%85.png?raw=true)

  

* 클래스

```java
private int data1;
	private String data2;
	private DataBean2 data3;
	private DataBean2 data4;

	public TestBean3(DataBean2 data3, DataBean2 data4) {
		this.data3 = data3;
		this.data4 = data4;
	}
```



* xml
* 생성자의 자동 주입 **autowire ="constructor"**

```xml
<!--생성자를 통한 자동주입 -->
	<bean id="obj5" class="kr.co.beans.TestBean3" autowire="constructor"/>
	
	<!--생성자에 자료형 2개만 받아서 기본자료형은 들어가지않아서 결과-->
	obj5.data1 : 0   인트 타입
 obj5.data2 : null   스트링 타입
 obj5.data3 : kr.co.beans.DataBean2@4b741d6d
 obj5.data4 : kr.co.beans.DataBean2@4b741d6d
```



* #### 자동주입은 참조자료형만 가능하기에 그외 자료형은 직접 명시적으로 적어야한다.

* xml
  * 자동 주입되는 객체는 안적어도 출력이된다.

```xml
<bean id="obj7" class="kr.co.beans.TestBean3" autowire="constructor">
	<constructor-arg value="200" index="0" type="int" />
	<constructor-arg value="문자열" index="1" type="String" />
	
	</bean>
```

* class

```java
public TestBean3(int data1, String data2, DataBean2 data3, DataBean2 data4) {
		this.data1 = data1;
		this.data2 = data2;
		this.data3 = data3;
		this.data4 = data4;
	}
```



* #### default 자동주입

  * autowire가 설정되어있지않는 bean들은 default 값을 설정할수있다.
  * autowire가 설정되있지않는 xml

```xml
<beans
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://www.springframework.org/schema/beans"
<!--맨 위의 값에 byname 등등 설정 가능하다-->
       default-autowire="byName">


<bean id="obj8" class="kr.co.beans.TestBean1"/>
```

* default값이 byname이라 같은 이름의 자료형이 들어가게 된다.



* #### default-autowire 적용받기 싫은 bean설정

```xml
<bean id="obj9" class="kr.co.beans.TestBean1" autowire="no"/>
```

* autowire 에 no값을 적으면 이 bean은 defaul-autowire에 예외가 된다.
