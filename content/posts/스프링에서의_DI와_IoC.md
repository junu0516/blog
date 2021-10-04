

+++
title = 'Spring DI와 IoC'
tags = ['spring', 'java']
categories = ['Spring']
date = "2021-09-20"

+++

Spring DI와 IoC의 개념에 대해 알아보자.

<br>  

![이러쿵저러쿵 :: 스프링(Spring) 프레임워크 기본 개념 강좌 (3) - IoC](https://t1.daumcdn.net/cfile/tistory/2434D33454E702B835)

<br>

## 1. DI(Dependency Injection, 의존성 주입)

__`DI(Dependency Injection)`__ 은 스프링 프레임워크의 의존성 주입을 일컫는 말로, 특정 객체를 미리 생성 후 주입하는 것을 말한다. 여기서 특정 클래스가 제 기능을 다하기 위해, 다른 클래스의 생성을 필요로하는 경우 클래스 간에  __`의존성`__ 이 존재한다고 표현한다.

아래의 코드를 통해 의존성에 대해 살펴보도록 하자

```java
class Car{
    Tire tire;
    
    public Car(){
        this.tire = new Tire();
    }
}
```

보통 의존 관계는 __`new`__ 키워드를 선언하는 것과 밀접한 관련이 있다. 위의 코드에서 Car 클래스는 Tire 클래스를 필요로 하며, 따라서 Car 클래스는 Tire 클래스에 의존성을 가진다고 할 수 있다.

하지만 위와 같은 코드는 우선 Car 클래스와 Tire 클래스 간의 결합도가 높기 때문에, __하나를 수정하면 다른 하나를 수정해야 하는 번거로움이 존재한다.__ 따라서, 결합도를 낮출수록(=의존성 혹은 종속성을 줄일수록) 코드의 재활용성 및 유연한 코드 작성이 가능해진다.     

의존성의 주입은 아래처럼 생성자나 setter 메소드를 통해서도 가능하다.  new를 통해 직접 생성하지 않고, 외부에서 생성한 객체를 가져와 인스턴스에 할당하는 것이다.

```java
class Car{
    Tire tire;
    
    public Car(Tire tire){
        this.tire = tire;
    }
    
    public void setTire(Tire tire){
        this.tire = tire;
    }
}
```

<br>

## 2. IoC(Inversion of Conrol)

스프링 프레임워크에서는 주입 대상이 되는 의존성 객체를 Bean이라고 한다. 처음 프로젝트가 실행될 때 Bean을 생성하여 Bean 컨테이너에서 관리하게 되고, 이후 필요시 컨테이너로부터 Bean을 주입받아 활용하는 것이다.

이러한 방식은 Bean 객체의 생성 및 소멸을 개발자가 아닌 프레임워크가 주도하게 함으로써, Singleton Pattern의 특징에서 알 수 있듯 빈번한 객체의 생성 및 소멸로 인한 자원 낭비를 줄이고 재사용성을 높인다는 이점이 있다.

이렇게 메소드나 객체의 호출을 개발자가 결정하지 않고, 프레임워크가 결정하는 것을 __`제어의 역전(Inversion of Control)`__ 이라고 표현한다. 스프링에서의 Bean이 만들어지고 실행되는 과정은 아래와 같다.

1. 최초 객체가 생성된다.
2. 의존성 객체가 주입된다.
   - 필드 인스턴스나 setter 혹은 생성자 등을 통해 주입
   - `@Autowired`  선언하여 프레임워크가 컨테이너에서, 선언된 객체 타입과 일치하는 Bean을 찾아 주입함
3. 의존성 객체 메소드가 호출된다.    

<br>

여기서 의존성 객체를 주입할 때 스프링에서 권장하는 방식은 __생성자방식__ 이다. 다양한 이유가 있겠지만, 공식문서에 의하면 주된 이유로 __순환참조로 인한 에러를 사전에 방지__ 할 수 있다는 점을 언급하고 있다.

아래의 코드를 통해 살펴보도록 하자.

```java
@Component
public class Dog{
    @Autowired
    private Cat cag;
    
    public void call(Car cat){
        System.out.println("Calling Cat");
        cat.call();
    }
}

@Component 
public class Cat{
    @Autowired
    private Dog dog;
    
    public void call(Dog dog){
        System.out.println("Calling Dog");
        dog.call();
    }
}
```

위와 같이 필드 주입 방식을 통해 두 개의 Bean이 서로 의존하는 상황에서, 하나의 Bean이 내무의 call() 메소드를 호출할 경우에는  CallStack이 지속적으로 쌓이면서 ``StackOverflowError``가 발생할 것이다. 쉽게 말해 Calling Cat -> Calling Dog -> Calling Cat -> Calling Dog이 무한으로 반복되는 상황인 것이다.

문제는 이러한 예외는 컴파일 에러가 아닌 __런타임 에러이기 때문에,__ 메소드가 실제로 호출되기 전까지는 순환참조된 부분을 찾아내기 힘들 수 있다. 만일, 생성자 주입 방식을 활용할 경우에는 이러한 순환참조 오류를 컴파일 시점에서 잡아낼 수 있기 때문에 생성자 주입 방식이 권장되는 것이다.

자바에서의 생성자 특성상 생성자 주입은 보통 __최초 생성자 호출시 1회 호출되는 것이 보장__ 되기 때문에, 순환참조가 일어날 경우 __`BeanCurrentlyInCreationException`__ 이 발생하여 사전에 문제를 방지할 수 있다.

```java
@Component
public class Dog{
    
    private Cat cag;
    
    @Autowired
    public Dog(Car cat){
        this.cat = cat;
    }
    
    public void call(Car cat){
        System.out.println("Calling Cat");
        cat.call();
    }
}

@Component 
public class Cat{
    
    private Dog dog;
    
    @Autowired
    public Cat(Dog dog){
        this.dog = dog;
    }
    
    public void call(Dog dog){
        System.out.println("Calling Dog");
        dog.call();
    }
}
```

위의 코드는 생성자 주입 방식을 적용한 것이다. 보통 단일 생성자만 존재하면 ``@Autowired``를 사용하지 않아도 되지만, 2개 이상인 경우에는 반드시 어노테이션을 선언해야 한다.

<br>

## 3. 스프링에서 Bean 호출해보기

이번에는 간단한 예제를 통해 스프링의 Bean Container에서 IoC가 어떻게 이루어지는지 살펴보도록 하자. 여기서는 JDK1.5 이후 사용 가능한 Java Config 클래스를 통해 Bean Container를 구현하였다.   

먼저, Bean으로 활용될 Car, Engine 두 개의 클래스를 아래와 같이 생성하였다.

```java
public class Car {
	private Engine v8;
	
	public Car() {
		System.out.println("Car 기본 생성자");
	}
	public void setEngine(Engine e) {
		this.v8 = e;
	}
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
public class Engine {
	public Engine() {
		System.out.println("Engine 기본 생성자");
	}
	public void exec() {
		System.out.println("엔진 동작");
	}
}
```

<br>

이후 Bean Container로 기능할 Java Config 클래스를 생성한다.

``` java
@Configuration //해당 어노테이션을 읽고 해당 클래스가 config 파일임을 인지(스프링 설정 클래스)
public class ApplicationConfig {
	/*
	 * @Bean 어노테이션이 붙은 메소드(생성자)를 실행해서,
	 * 리턴값으로 받은 객체들을 자동으로 싱글톤으로 관리해줌
	 * pom.xml에서 일일히 등록하는 것보다 더욱 편리!!
	 * 
	 * */
	
	@Bean
	public Car car(Engine e) { //클래스명과 실행파일에서의 getBean()의 매개변수를 서로 맞춰주도록 함
		Car c = new Car();
		//c.setEngine(e);
		return c;
	}
	
	@Bean
	public Engine engine() {
		return new Engine();
	}	
}
```

위의 코드에서 __`@Configuration`__ 어노테이션은, 간단히 말해 ApplicationConfig 클래스가 Bean Container로 기능할 것임을 나타내는 어노테이션이다. 해당 클래스 내에서  __`@Bean`__ 어노테이션이 선언된 메소드들이 리턴하는 객체가 컨테이너가 가진 Bean이 된다.  

이후에는 실행 메소드에서 직접 컨테이너를 생성해서 Bean을 주입해보도록 하자.

```java
public class ApplicationContextTest {
	public static void main(String[] args) {
		//컨테이너 생성
        ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		
        //컨테이너에서 Bean 호출
		Car car = (Car)ac.getBean("car");
		car.run();
		
	}
}
```

여기서는 컨테이너의 구현체로 __`AnnotationConfigApplicationContext`__ 클래스를 사용하였고, 매개변수로 앞서 생성한 __ApplicationConfig__ 클래스를 넣어주었다. 

이후 메인 메소드가 호출되면, AnnotationConfigApplicationContext는 생성자로 주입받은 config 클래스를 읽어 IoC를 적용하게 되며, config 클래스 내에 선언된 여러 Bean들이 싱글톤 방식으로 관리된다.

<br>

---

## Reference

- [위키피디아 의존성 주입](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85)
- [Constructor Dependency Injection in Spring](https://www.baeldung.com/constructor-injection-in-spring)
- [[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)