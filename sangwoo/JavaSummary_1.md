- <h1>1장 기본 프로그래밍 구조</h1>
    <br>
    <h2>1.1 첫 번째 프로그래밍</h2>
    
    ---
    
    <h3>1.1.1 “Hello, World” 프로그램 파헤치기</h3>
    
    ---
    
    ```java
    package ch01.sec01;
    
    // 첫 번째 자바 프로그램
    
    public class HelloWorld {
        public static void main(String[] args){
            System.out.println("Hello, World!");
        }
    }
    ```
    ---
    자바의 특징
    <blockquote>
    자바의 가장 큰 특징은 객체 지향 언어라는 것이다. 따라서 프로그램에서 객체(Object)를 조작해 일을 킨다. 각 객체는 특정 클래스(Class)에 속하며, 그 객체를 클래스의 인스턴스(Instance)라고 한다. 클래스의 객체 상태와 할 수 있는 일을 정의하며 자바는 모든 코드를 클래스 안에서 정의한다. 위 코드를 보면 main 함수가 HelloWorld라는 클래스 안에서 정의되고 실행되는 것을 볼 수 있다.
    </blockquote>
    <br>
    main Method
    <blockquote>
    main 함수는 메서드(Method)이다(== 클래스 안에서 선언된 함수). main 메서드는 프로그램을 실행할 때, 첫 번째로 호출하는 메서드이다. 이 메서드는 객체가 없어도 작동하도록 static 으로 선언한다. 여기서 main이 호출될 시엔는 미리 정의된 소수의 객체만 존재하며 이 중 어느것도 HelloWorld 클래스의 인스턴스가 아니다. void는 값을 반환하지 않는다는 뜻이다.<br><br>
    main 메서드의 바디를 살펴보자. ex) System.out.println은 System.out으로 메시지를 표시하는 명령 한 줄로 구성된다. System.out은 자바 프로글매에서 ‘표준 출력(Standard output)’을 나타내는 객체이다.
    </blockquote>
    <br>
    public, private
    <blockquote>
    자바는 많은 기능을 public이나 private로 선언한다. 위 코드에서는 HelloWorld 클래스와 main 메서드를 public으로 선언했다. 보통 클래스와 패키지는 관련 있는 클래스를 모아 놓은 집합이다. 관련 있는 클래스들을 패키지 안에 넣어 관리하면 이름이 같은 클래스가 생기더라도 충돌을 막을 수 있다. HelloWorld 클래스의 전체 이름은 (패키지이름).HelloWorld 이 된다.
    </blockquote>
    <br>
    주석
    <blockquote>
    주석은 //로 시작한다. //부터 줄의 끝까지 있는 모든 문자는 컴파일러가 무시한다.(개발자들이 보기 위함), /*로 시작하고 */로 끝내면 엔터 이후에도 이 사이에 있는 모든 글은 주석으로 처리된다.
    </blockquote>
    <br>
    자바의 사용
    <blockquote>
    자바는 빠르게 실행하기 위한 스크립팅 언어는 아니다. 클래스, 패키지, 모듈로 구조화해 대규모 프로그램을 개발하기 좋은 언어이다.
    <br><br>
    자바 언어는 아주 간결하고 일관성이 있다. 몇몇 언어는 클래스 내부에 선언하는 변수와 메서드 외에 전역 변수와 전역 함수도 있지만, 자바는 모든 것을 클래스 안에 선언한다. 이러한 일관성으로 어색하지만 프로그램의 의미는 쉽게 이해할 수 있다.
    </blockquote>
    <br>
    <br>
    <h3>1.1.2 자바 프로그램 컴파일 및 실행</h3>
    
    ---
    
    자바 프로그램을 컴파일하고 실행하기 위해서는 JDK(Java Development Kit, 자바 개발 키트)를 설치해야 한다. 
    
    JDK를 설치했다면 터미널(명령 프롬프트)를 연다. 소스코드를 작성해 놓은 디렉터리로 이동해서 다음 명령을 실행한다.
    
    ```java
    $ jacac ch01/sec01/HelloWorld.java
    
    $ java ch01.sec01.HelloWorld
    
    // 실행결과 : HelloWorld
    ```

    자바 프로그램은 두 단계를 거쳐 실행한다. javac 명령으로 자바 소스 코드를 특정 기계에 종속되지 않은 중간 표현인 바이트 코드(byte code)로 컴파일(compile)해서 클래스 파일(class file)에 저장한다. java 명령으로 가상 머신(virtual machine)을 구동하고 클래스 파일을 로드해서 바이트 코드를 실행한다.
   

    바이트 코드는 한번 컴파일하면 모든 자바 가상 머신(Java Virtual machine)에서 실행할 수 있다. “한 번 작성하고, 어디서나 실행한다.” “Write once, run anywhere”. 는 자바의 중요한 설계 기준이였다.

    javac 컴파일러는 파일(file) 이름을 실행한다. 이때 경로는 슬러시(/)로 구분하고 확장자 .java를 붙인다. 그 후 java 가상 머신 런처는 클래스(class)이름으로 실행한다. 이때 패키지는 점(.)으로 구분하고 확장자는 붙이지 않는다.
    <br>
    <br>
    <h3>1.1.3 메서드 호출</h3>

    ---

    ```java
    System.out.println(”HelloWorld”); 
    // object.methodName(arguments) 
    ```
    <blockquote>
    System.out은 객체이며, PrintStream 클래스의 인스턴스이다. PrintStream 클래스에는 println, print 등 메서드가 존재하고, 이런 메서드는 해당 클래스의 객체(인스턴스)에서 작동하므로 인스턴스 메소드(Instance Method)라고 한다. 객체의 인스턴스에는 점(.) 표기법(dot notation)을 사용해야 한다.
    <br>String 클래스에는 String 길이의 객체를 반환하는 length 메서드가 존재한다.
    </blockquote>
    <br>

    ```java
    System.out.println(”HelloWorld”.length()); 

    // 10
    ```
    <blockquote>
    length는 인수를 받지 않음. println 메서드와 다르게 length 메서드는 결과를 반환한다. 따라서 사용할 수 있는 유일한 방법은 출력하는 것이다.

    자바는 객체 대부분을 생성(construct)해야 한. System.out 과 “Hello, World!” 객체는 바로 사용할 수 있게 미리 준비된 객체이다.
    </blockquote>
    <br>

    <h3>1.1.4 JShell 실행 </h3>

    ---

    <blockquote>
    JShell은 자바의 또 다른 실행 방법이다. JShell 프로그램은 “REPL(Read-Evaluate-Print-Loop)”를 제공한다. 즉, 자바 표현식을 입력하면 입력을 평가해 결과를 출력하고 다음 입력을 기다린다. 터미널시에 jshell을 입력하고 실행시키면 된다. 루프를 돌기 때문에 자바 표현식 입력을 기다리고, 입력시에 입력을 평가해 결과를 출력하고 다음 입력을 기다리게 된다(여기서는 System.out.println을 입력하지 않았다.) .
    <br> 
    또한 출력 결과물을 보면 
    $1 ==>13 이렇게 나오는데 이는 13의 값을 $1로 접근할 수 있다는 말이다.
    
    자바를 스크립트 언어처럼 간단하게 사용해 볼 수 있게 해주는 출력 도구이지만, 실제 개발을 할 때에 쓰이긴 어렵다.
    </blockquote>