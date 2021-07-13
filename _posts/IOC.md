#  IOC

* 제어의 역전
  * 객체에 대한 제어권을 개발자가아닌 프레임워크가 가지는것을 의미합니다.
  * 프레임워크가 객체를 생성 반환하여 코드가 동작하는 순서를 프레임워크가 결정합니다



* ### POJO Class

  * 자바 모델 기능 프레임워크에 따르지않고 독립이고 단순한 기능만 가진 java객체
  * 이러한 객체들을 Bean이라 부름



* ### IoC 컨테이너의 종류

  * #### BeanFactory

    * 클래스를 통해 객체를 생성하고 이를 전달한다. (주 이용)
    * 상속 등 객체간의 관계를 형성 관리
    * Bean에 관련된 설정을 위한 xml 파일은 즉시 로딩하지만 객체는 개발자가 요구할때 생성한다.
    * ClassPathResource
    * FIleSystemResource
    * XmlBeanFactory

    

  * #### ApplicationContext
  
    * 클래스를 통해 객체를 생성하고 이를 전달한다.
    * 상속 등 객체 간의 관계를 형성하고 관리한다.
    * 국제화 지원등 문자열 관련 기능 제공
    * 리스너로 등록되어 있는 Bean에 이벤트 발생
    * Bean에 관련된 설정을 위한 xml 파일을 즉시 로딩하며 객체를 미리 생성해서 가지고 있다
    * ClassPathXmlApplicationContext
    * FileSystemXmlApplicationContext
    * XmlWebApplicationContext



* ### BeanFactory 사용법

  * main 메서드속

```java
	//BeanFactory 사용 Deprecated 되엇습니다. 3.대만 가능
	//패키지 내부에 있는 beans.xml 이용
	//패키지 내부 xml 사용위해서는 ClassPathResource (경로)
	public static void test1() {
		//
//		xml파일을 로딩할때 xml에 저장되어있는 객체를 자동으로 생성하지않는다.
		ClassPathResource res = new 	         ClassPathResource("kr/co/config/beans.xml");
		XmlBeanFactory factory = new XmlBeanFactory(res);
		
		//factory는 getbean 메서드를 가져올때 객체를 생성합니다.
//        getBean(beans.xml파일속 아이디 이름 , 객체명.class)
		TestBean t1 = factory.getBean("t1",TestBean.class);
		System.out.printf("t1 : %s\n",t1);
		
		//같은 객체를 가져오면 컨테이너는 새로운 객체를 생성하는게 아니라 객체를 가지고 보관하고있다가
		//생성된 주소값을 가져온다 위 아래 같은 객체
		TestBean t2 = factory.getBean("t1",TestBean.class);
		System.out.printf("t2 : %s\n",t2);
```





* ### Bean 객체 생성하기

  * 태그의 기본속성

    | class     | 객체를 생성하기 위해 사용할 클래스 지정 (필)                 |
    | --------- | ------------------------------------------------------------ |
    | id        | bean 객체를 가져오기 위해 사용되는 이름 지정                 |
    | lazy-init | 싱글톤인 경우 xml 로딩할때 객체 생성 여부를 설정<br />true : xml 로딩시 생성하지 않고 객체를 가져옴 |
    | scope     | 객체의 범위 설정<br />singleton : 객체를 하나만 생성해 가져옴<br />prototype : 객체를 가져올때 마다 객체생성 |

    

  * Spring에서는 사용할 객체를 bean configuration (xml)파일에 정의하면 된다.



* ### Bean객체의 생명주기

  * ### Spring의 Bean은 다음과 같은 상황일 때 객체가 생성된다
    * Singleton인 경우  xml 파일을 로딩할 때 객체가 생성된다.
    * Singleton이고 lazy-init 속성이 true일 경우 getBean 메서드를 사용할 때 객체가 생성된다.
    * prototype일 경우 getBean 메서드를 사용할 때 객체가 생성된다.

    ```java
    ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("kr/co/config/beans.xml"); 
    //ClassPath 객체를 만드는 부분이 xml을 로딩하는 부분이다.
    //아무것도 설정안하면 default가 싱글톤이기에 바로 객체가 생성이 되고 
    //lazy-init 에 true이면 getBean을 할때 객체가 생성이된다.
    //scope가 prototype일 경우에는 객체를 가져올때마다 객체가 생성이된다.
    
    ctx.close(); // <-ioc 컨테이너 사용 안하는것을 의미 이때 객체가 소멸이된다.
    ```

    * **beans.xml 객체 설정**

    ```java
    <bean id="t1" class="kr.co.beans.TestBean1" lazy-init="true"/>
        //lazy-init = true값이라 getbean할때만 객체가 생성이된다.
    ```

    * **getBean설정으로 객체 생성**

    ```java
    ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("kr/co/config/beans.xml");
    		
    		TestBean1 t1 = ctx.getBean("t1",TestBean1.class);
    		ctx.close();
    ```

    

  

  * ### Spring Bean은 다음과 같은 상황일때 객체가 소멸된다.

    * IoC컨테이너가 종료 때 객체가 소멸된다.
    
* ### 객체 생성과 소멸시 호출될 메서드 등록
    * 객체가 생성되면 가장 먼저 생성자가 호출된다.
  
  ```java
    //클래스에 메서드 설정
    public class TestBean1 {
    
    	public TestBean1() {
    		System.out.println("TestBean1의 생성자");
    	}
    	
    	public void bean1_init() {
    		System.out.println("TestBean1의 init 메서드");
    	}
    }
    
    //bean.xml에 init-method 설정
    <bean id="t1" class="kr.co.beans.TestBean1" lazy-init="true" init-method="bean1_init"/>
    ```
  
  
  
  * init-method : 생성자가 호출 이후 자동으로 호출된다.
  
  ```java
    동일하게 xml에 작성하면
    ctx.close(); ioc컨테이너를 사용안할때 불리게 된다.
    ```
  
  
  
  * destroy-method : 객체가 소멸될 때 자동으로 호출된다. 
    * default-init-method : init-method를 설정하지 않은 경우 자동으로 호출된다.
    * default-destroy-method : destroy-method를 설정하지 않은 경우 자동으로 호출된다.
  
  ```java
    default 메서드는 init method , destroy-method가 적용이 안되있으면 호출이된다.
    
    <beans default-init-method="default_init" default-destroy-method="default_destroy">
    	
    	<bean id="t1" class="kr.co.beans.TestBean1" lazy-init="true" init-method="bean1_init" destroy-method="bean1_destroy"/>
    	<bean id="t2" class="kr.co.beans.TestBean2" lazy-init="true"  />
    	
    </beans> 
    ```
  
  * 위치는 bean이 아니라 <beans 에 작성을 한다.
  
* ### bean.xml 에 method 작성과 default 메서드 작성시 호출 메서드
  
  ```java
    //객체가 생성될때 생성자가 호출된 이후 init-method에 설정한 메서드가 자동으로 호출되고 IoC컨테이너의 close 메서드를 호출하면 객체가 소멸되며 destroy-method에 설정한 메서드가 자동으로 호출된다.
    <bean id="t1" class="kr.co.beans.TestBean1" lazy-init="true" init-method="bean1_init" destroy-method="bean1_destroy"/>
    
    //init-mehtod와 destroy-method가 설정되있지 않다면 default-init-method와 , destroy-init-method에 설정되엉 있는 메서드를 호출한다.
    <bean id="t2" class="kr.co.beans.TestBean2" lazy-init="true"  />
    
    //init-method, destroy-method에 설정되어 있는 메서드가 호출된다.    
    	<bean id="t3" class="kr.co.beans.TestBean3" lazy-init="true" init-method="bean3_init" destroy-method="bean3_destroy"/>
    	
    //init-method default method 아무것도 가지고 있지않는 클래스일경우 아무것도 호출되지 않는다.
    <bean id="t4" class="kr.co.beans.TestBean4" lazy-init="true"/>
        //class안에 default 메서드도 정의되있지않음
    ```
  
  * 즉, default 메서드보다 init-method 본인에 설정한 method가 우선이되어 호출되고 default-method는 init-method가 설정되어있으면 나오지 않는다.



* ### BeanPostProcesseor

  * Bean 객체를 정의할 때 init-method 속성을 설정하면 객체가 생성될 때 자동으로 호출될 메서드를 지정할 수 있다.

  * 이 때 BeanPostProcessor 인터페이스를 구현한 클래스를 정의하면 Bean 객체를 생성할 때 호출될 init 메서드 호출을 가로채 다른 메서드를 호출 될수 있도록 할 수 잇다. (공통으로 처리할 일 가로채서 작업을 함)

  * #### 메서드

| postProcessBeforeInitialization | init-method에 지정된 메서드가 호출되기 전에 호출된다 |
| ------------------------------- | ---------------------------------------------------- |
| postProcessAfterInitializtion   | init-method에 지정된 메서드가 호출된 후에 호출된다   |

* init-method가 지정되어 있지 않더라도 자동으로 호출된다.

```java
//class에 따로 BeanPostProcessor를 구현하여 정의 하면 된다.
//구현한 클래스는 따로 getBean 하여 만들지 않아도 된다.
public class TestBeanProsProcessor implements BeanPostProcessor{
//매개변수 Object bean에 객체에 메모리 위치가 담겨들어와서 미리 호출된다.
	//init-method 호출전 
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("before");
		return bean;
	}
	
	//init-method 호출 후
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("after");
		return bean;
	}
}
```

* bean.xml에 bean을 정의한다
* 따로 작성하는게 아니라 bean 클래스만 정의하면 spring에서 사용하여줍니다.

```xml
	<bean id='t1' class='kr.co.beans.Test1' lazy-init="true" init-method="bean1_init"/>
	<bean id ='t2' class='kr.co.beans.TestBean2' lazy-init="true" init-method="bean2_init"/>
<!-- postProcess bean 태그로 클래스만 정의하면 init 메서드 사용전 후 스프링에서 사용해줌
만약 위 bean 에 init이 정의되있지 않아도 마지막에 메서드는 호출이된다.-->
	<bean class='kr.co.processor.TestBeanProsProcessor'/>
</beans>
```



* 메소드 전 후 호출 객체마다 다르게 활성화 시키고 싶다면 구현한 메서드 **String beanName** 이용

```java
public class TestBeanProsProcessor implements BeanPostProcessor{

	//init-method 호출전
    //매개변수 String beanName에 빈 클래스 이름이 들어옴
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("before");
		switch(beanName) {
		case "t1" :
			System.out.println("id가 t1인 bean 객체 생성");
			break;
		case "t2":
			System.out.println("id가 t2인 bean 객체 생성");
		}
		
		return bean;
```



