# 1-1 첫 번째 프로그램

### 1.1.1 “Hello, World” 프로그램 파헤치기

```java
package ch01.sec01;

// 첫 번째 자바 프로그램

public class HelloWorld {
    public static void main(String[] args){
				System.out.println("Hellow, World !");
		}
}
```

- Hello World 코드 : main은 메소드(method)로 클래스 안에 선언된 함수이다.
    - main 메소드는 프로그램을 실행 할 때 첫 번째로 호출하는 메소드
    - 객체가 없어도 작동하도록 static을 선언
    - 값을 반환하지 않아 void로 선언

- 자바는 객체 지향 언어, 객체를 조작한다.
- 특정 클래스(class)에 속하는 객체(object) = 클래스(class)의 인스턴스(instance)

- 가시성 레벨 4가지
    - public
    - private
    - 

- 패키지 : 관련있는 클래스를 모은 집합 → 관련있는 클래스를 모아 서로 충돌하지 않도록 하는게 좋음
- 주석 : // 으로 //이 시작되는 부분부터 그 줄 끝까지 주석처리, /* */ 사이는 모두 주석처리 ( 문서화 주석 )
- 표준출력 : System.out 객체

### 1.1.2 자바 프로그램 컴파일 및 실행

- 컴파일 및 샐행환경
    
    JDK(Java Development Kit) 자바 개발 키트
    
    IDE(Integrated Development Environment) 통합 개발 환경
    
- 자바 프로그램의 실행
    - Javac 명령으로 자바 소스코드를 중간표현(특정 기계에 종속되지 않은 표현)인 바이트 코드로 컴파일 후 클래스파일로 저장
    - java 명령으로 가상머신을 구동하고 클래스파일 로드 후 바이트 코드 실행
    - 바이트 코드는 한번 컴파일하면 모든 가상머신에서 사용가능 → 한번 작성하고 어디서든 실행 가능
    - javac 컴파일러는 파일 이름으로 실행
        - 경로는 /로 구분 .Java를 붙인다
    - java 가상 머신 런처는 클래스 이름으로 실행
        - 패키지는 . 으로 구분, 확장자는 붙이지 않음

### 1.1.3 메소드 호출

- System.out = 객체, PrintStream 클래스의 인스턴스
- PrintStream 클래스에는 println, print 메소드 등이 있다
    - 위와같은 메서드는 해당 클래스의 객체 (인스턴스) 에서 작동하여 객체 메서드 ( 인스턴스 메서드 )라고 한다

- 자바는 객체 대부분을 **생성**해야한다
```java
Random generater = new Random(); // 생성 연산자 new를 사용해 랜덤 클래스 생성
System.out.println(generater.nextint());
```