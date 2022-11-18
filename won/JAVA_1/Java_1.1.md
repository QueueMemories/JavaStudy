1. 첫 번째 프로그램

**자바**는 객체 지향 언어이므로 대부분 **객체(object)**를 조작해 일을 시킨다. 각 객체는 특정 **클래스(class)**에 속하며, 그 객체를 클래스의 **인스턴스(instance)**라고 한다.

```java
package ch01.sec01;

//첫 번째 자바 프로그램

public class HelloWorld {
    public static void main(String[] args){
        System.out.println("Hello, World!");
    }
}
```

main은 **메서드(method)**다. 즉, 클래스 안에 선언된 함수다. main 메서드는 프로그램을 실행할때 첫번째로 호출하는 메서드다. 객체가 없어도 작동하도록 static으로 선언한다. 또 값을 반환하지 않으므로 **void**로 선언했다.

- 자바는 **public**이나 **private**으로 선언하는데 지정할 수 있는 가시성 레벨이 두가지 더 있다. 클래스와 메서드는 보통 **public**으로 선언한다.
- **패키지(package)**는 관련 있는 클래스를 모아 놓은 집합이다. 클래스를 패키지 안에 넣어 관련 있는 클래스들을 함께 묶어서 이름이 같은 클래스가 여러 개 있더라도 서로 충돌하지 않게 하면 좋다.

패키지이름은 장과 절 번호 소스코드로 사용하면 좋다. 따라서 HelloWolrd 클래스의 전체 이름은 **ch01.sec01.HelloWolrd**가 된다.

- 주석은 한줄은 “**//”** 여러줄은 “**/*”** 와 “***/”**로 넣을수있다.

자바는 몇 가지 명령을 빠르게 실행하려고 사용하는 스크립팅 언어가 아니다. 클래스, 패키지, 모듈로 **구조화**해 **대규모 프로그램**을 개발하기 **좋은** 언어다. 자바 언어는 아주 **간결**하고 **일관성**있다. 그리고 자바는 **모든것**을 클래스안에 선언한다.

# 2. 자바 프로그램 컴파일 및 실행

자바는 **javac** 명령으로 자바 소스코드를 특정 기계에 종속되지 않은 중간 표현인 **바이트 코드(byte code)**로 **컴파일(compile)**해서 **클래스 파일(class file)**에 저장한다. 

javac(자바컴파일러)는 소스파일(.java)을 바이트 코드(.class) 타입으로 변환

 **Java → Virtual machine → class file → byte code 실행**

- **JDK(java Development kit)**(자바 개발 키트): 자바 프로그램을 컴파일하고 실행
- **IDE(Integrated Development Environment)**(통합 개발 환경) (ex. 이클립스)

# 3. 메서드 호출

```java
System.*out*.println("Hello, World!");
```

System.out: 객체이며, PrintStream 클래스의 인스턴스이다.

PrintStream: println, print등 메서드가 있다.

이런 메서드는 해당 클래스의 객체(인스턴스)에서 작동하므로 인스턴스메서드(instance method)라고한다.

객체의 인스턴스 메서드를 호출하려면 점(.)표기법(dot notation)을 사용해야 한다.

자바는 객체 대부분을 생성(construct)해야한다. ex. new Random

```java
new Random().nextInt()
```

클래스 이름 뒤에는 생성 인수 목록이 온다. 생성된 객체의 메서드를 다음과 같이 호출할수있다. ex. ).nextInt()

한 객체에서 메서드를 두 번 이상 호출하려면 해당 객체를 변수에 저장해야 한다.

Random 클래스는 java.util 패키지에 선언되어 있다. 프로그램에서 Random 클래스를 사용하려면 다음과 같이 import 문을 추가해야 한다.

# 4. JShell 실행

JShell 프로그램은 ‘REPL(Read-Evaluate-PrintLoop)(읽기-평가 -출력루프)를 제공한다. 따라서 강력한 개발 환경을 띄우거나 public static void main을 사용하지 않고도 자바 언어와 라이브러리를 쉽고 재미있게 배울수있는 장점이있다.