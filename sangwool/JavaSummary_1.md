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
        <br><br>
    <h2>1.2 기본 타입</h2>

    ---
    <blockquote>
    자바는 객체 지향 언어이지만, 그렇다고 해서 자바의 모든 값이 객체는 아니다. 몇 가지 값은 기본 타입(primitice type) 에 속한다. 기본 타입 중 네 가지(byte, short, int, long)는 부호 있는 정수 타입이고, 두 가지(float, double) 는 부동소수점 타입, 하나는 문자열 인코딩에 사용하는 문자 타입인 char, 나머지 하나는 진릿값을 나타내는 boolean 타입이다.
    </blockquote>
    <br>
    기본 타입 범위.
    <table border="1">
	<th>타입</th>
	<th>저장공간</th>
    <th>범위(포함)</th>
	<tr>
	    <td>byte</td>
	    <td>1바이트</td>
        <td>-128 ~ 127</td>
	</tr>
	<tr>
	    <td>short</td>
	    <td>2바이트</td>
        <td>-32.768~32.767</td>
	</tr>
    <tr>
    	<td>int</td>
	    <td>3바이트</td>
        <td>-2.147,483,648~2,147.483,647(20억이 넘음)</td>
    </tr>
    <tr>
    	<td>long</td>
	    <td>4바이트</td>
        <td>-9,223,372.036,854,775,808~9,223,372,036,854,775,807</td>
    </tr>
    </table>
    <br>
    <h3>1.2.1 부호 있는 정수 타입</h3>

    ---
    <blockquote>
    Integer.MIN_VAlUE 상수는 가장 작은 정수 값, Integer.MAX_VALUE는 가장 큰 정수 값을 나타낸다. Long, Short, Byte 클래스에도 MIN_VALUE와 MIN_VALUE 상수가 있다.
    <br>if) long 타입보다 큰 수가 있을 때는, BigInteger 클래스를 사용하여 처리한다.
    </blockquote>
    <br>
    자바 정수 타입 범위
    <blockquote>
    프로그램을 실행하는 머신과 상관없이 동일하다. C나 C++로 작성한 프로그램은 컴파일하는 프로세서에 따라 정수 타입이 다르다.
    </blockquote>
    <br>
    진수 표기법
    <blockquote>
    long 정수 리터럴은 접미어 L을 붙여서 작성한다(400L). byte나 short 타입 리터럴을 작성하는 문법은 없다. 따라서 캐스트 표기법을 사용한다.
    16진수는 0x를 붙여 작성. 2진수는 0b를 붙여서 작성한다.
    8진수는 0 을 붙여 사용한다. 011 은 9 이렇게 하지만 이런 형태는 혼동을 줄 수도 있으니 가급적 사용하지 않는게 좋다.
    숫자 리터럴은 밑줄()을 붙일 수 있다. 백만은 1_000_000 또는 0b1111_0100_0010_0100_0000 이렇게 나타낼 수 있다.
    </blockquote>
    <br><br>
    <h3>1.2.2 부동소수점 타입</h3>

    ---

    부동 소수점 타입은 소수점 부분이 있는 숫자를 나타낸다.<br><br>
    부동 소수점 범위.
    <table border="1">
	<th>타입</th>
	<th>저장공간</th>
    <th>범위</th>
	<tr>
	    <td>float</td>
	    <td>4바이트</td>
        <td>약 43 40282347E+38F(유효자릿수 6~7)</td>
	</tr>
	<tr>
	    <td>double</td>
	    <td>8바이트</td>
        <td>약 ±179769313486231570E+308(유효자릿수 15)</td>
	</tr>
    </table>
    <br>
    float 사용법<br>
    <blockquote>
    float 는 4바이트이고, 사용하기 위해서는 리터럴에 접미어 F를 붙어야 한다. 붙이지 않으면 자동으로 double 타입이 된다.
    </blockquote><br>

    Double 속성의 예외<br>
    <blockquote>
    무한대를 나타내는 Double.POSITIVE_INFINITY, 음의 무한대를 나타내는 Double.NEGATIVE_INFINITY, 숫자가 아니라는 것 을 나타내는 Double.NaN 등 특별한 부동소수점 값이 있다.
    </blockquote><br>

    비교하는 방법의 예외 처리<br>
    <blockquote>
    숫자가 아닌 값을 검사할 때는 if (x == Double.NaN) 으로 작성해서는 x가 NaN인지 검사할 수 없다. if (Double.isNaN(x))로 작성해야 검사할 수 있다. Double.isInfinite로는 무한대를 테스트하고, Double.isFinite로는 부동소수점 수가 무한대도 아니고 NaN도 아닌지 검사할 수 있다.<br>
    </blockquote><br>
    <br>
    부동 소수점 수는 금융 계산에는 적합하지 않다. 금융 계산은 반올림 오류를 용납하지 않기 때문이다.

    ```java
    System.out.println(2.0 - 1.1)

    // console : 0.8999999999999999 를 출력.
    ```
    <blockquote>
    위의 경우, 0.9가 아닌 다른 값을 출력한다. 따라서 임의의 정밀도로 반올림 오류가 없는 정확한 숫자 계산이 필요할 때는 부동 소수점 수를 사용하지 않고, BigDecimal 클래스를 사용해야 한다.
    </blockquote><br><br>

    <h3>1.2.3 char 타입</h3>
    
    ---

    <blockquote>
    char 타입은 자바가 사용하는 UTF-16 문자 인코딩의 ‘코드 유닛(code unit)’을 나타낸다.

    ex) ‘J’ 는 값이 74(16진수로는 4A)인 문자 리터럴로 유니코드 문자 ‘U+004A(라틴 대문자 J)’를 나타내는 코드 유닛이다. 접두어 \u를 사용해 코드 유닛을 16진수로 나타낼 수도 있다(’\u263A’는 ‘J’).

    특수 코드 ‘\n’, ‘\r’, ‘\t’, ‘\b’ 는 각각 라인피드 (line feed), 캐리지 리턴(carriage return) - 출력 위치를 맨 앞으로 이동, 탭(tab), 백스페이스(backspace)를 나타낸다.

    ‘’ 와 \ 를 나타내기 위해서는 \로 이스케이프(escape)한다(\’ ,  \\). 
    </blockquote><br><br>

    <h3>1.2.4 boolean 타입</h3>
    
    ---

    <blockquote>
    boolean 타입 값은 false와 true 두 개뿐이다. 자바에서 boolean은 숫자 타입이 아니고 boolean값과 정수 0, 1사이에는 아무 관련이 없다.
    </blockquote>
    <br><br>
    <h2>1.3 변수</h2>

    ---

    
    변수와 상수를 선언하고 초기화하는 법을 배운다.<br><br>
    <h3>1.3.1 변수 선언</h3>

    ---

    자바는 타입 결합이 강한 언어디므로 각 변수에는 해당 타입 값만 저장할 수 있다. 변수를 선언할 때는 타입과 이름을 지정해야 하고, 필요하면 초깃값을 지정할 수도 있다.

    ```java
    int total = 0;

    // 타입이 같은 변수 여러 개를 한 문장으로 선언 가능.

    int total = 0, count

    // 여기서 count는 초기화되지 않은 정수이다.

    Random generator = new Random();

    // 객체의 클래스 이름을 두 번 사용했다. 첫 번째 Random 은 generator 변수의 타입이고,
    // 두번째 Random 은 이 클래스의 객체를 생성하는 new 표현식의 일부이다.
    ```
    <br>
    <h3>1.3.2 변수 이름</h3>

    ---
    - 변수 이름은 반드시 문자로 시작. (메서드와 클래스도 동일)<br>
    - 변수 이름은 문자, 숫자, 기호 _와 \$로 구성 가능, 하지만 \$는 자동으로 생성되는 이름이기에 직접 이름을 지을 때는 사용하지 않음. 자바 9부터는 _자체는 명령어로 등록되어 단독 사용시에 오류 발생. <br>
    - 문자와 숫자는 라틴 알파벳에 한정되지 않아 모든 알파벳으로 구성 가능. 또한, 자바는 대, 소문자를 구분하기에 count와 Count는 다른 이름으로 인식됨. 이름에는 공백과 기호가 사용 불가하고, double처럼 예약어(명령어)도 사용이 불가능.<br>
    - 일반적으로 변수와 메서드의 이름은 소문자로 시작, 클래스 이름은 대문자로 시작. 자바 프로그래머는  낙타 표기법을 선호하며 그 정의는 countOfInvalidInputs처럼  여러 단어로 구성되어 있을 때, 각 단어의 첫 글자를 대문자로 쓰는 것을 의미함.


    <br><br>
    <h3>1.3.3 변수 초기화</h3>

    ---

    메서드 안에 변수를 선언했다면, 해당 변수를 반드시 초기화한 후에 사용해야 한다.

    ```java
    int count;
    count++; // 오류 발생 - 초기화되지 않은 변수 사용.

    int count;
    int (total == 0) {
        count = 0;
    } else {
        count++; // 오류 발생 - 초기화되지 않은 변수 사용.
    }

    // 컴파일러는 변수를 사용하기 전에 초기화했는지 검증한다.
    // 위의 두번째 코드는 total이 0일시에 오류가 나지 않지만, 
    // 컴파일러는 else문까지도 미리 탐색하기에 오류가 발생한다.

    Scanner in = new Scanner(System.in); 
    System.out.println("How old are you?");
    int age = in.nextInt();

    // 위 코드는 변수를 변수의 초깃값으로 사용할 값을 얻는 시점에 선언했다.
    // 이처럼 변수를 사용하기 직전까지 변수 선언 시기를 늦추는 것이 좋은 방식이다.
    ```
    <br>

    <h3>1.3.4 상수</h3>

    ---

    final 키워드는 한번 할당하면 변경할 수 없는 값에 사용한다. 다른 언어에서는 이 값을 상수(constant)라고 한다.

    ```java
    final int DAYS_PER_WEEK = 7;

    // 보통 상수 이름은 전부 대문자로 선언.

    public class Calendar {
        public static final int DAYS_PER_WEEK = 7;
    }
    // 위처럼 static 키워드로 상수를 메서드 외부에 선언할 수도 있다.
    // 그렇게 되면, Calendar 내부에서는 이 상수를 DAYS_PER_WEEK로 참조하고,
    // 이 상수는 여러 메서드에서 사용할 수 있다.
    // 다른 클래스에서 참조하기 위해서는 Calendar.DAYS_PER_WEEK 로 접근한다.
    ```
    <blockquote>
    Tip.

    System 클래스에는 다음 상수가 선언되어 있기에 어디서나 System.out으로 사용될 수 있다.
    public static final PrintStream out (out은 PrintStream 타입으로 선언),
    out은 상수이지만, 대문자를 쓰지 않는다.
    </blockquote>
    

    ```java
    final int DAYS_IN_FEBRUARY;
    if (leapYear) {
        DAYS_IN_FEBRUARY = 29;
    } else {
        DAYS_IN_FEBRUARY = 28;
    }
    // 일단 값을 할당하면 최종 값이 되어 절대로 변경할 수 없다. 
    ```

    final 변수는 사용 전에 한번만 초기화 하면, 초기화를 나중으로 미룰 수 있다. 이래서 ‘최종’ 변수라는 이름이 붙었다. 위는 final 초기화 규칙을 만족하는 코드이다.
    
    <br>
    <blockquote>
    Tip. (열거타입)

    서로 관련 있는 상수의 집합이 필요할 때 사용하는 enumerated type을 정의하면 된다.

    ```java
    public static final int MONDAY = 0;
    public static final int TUESDAY = 1;
    public static final int WEDNESDAY = 2;
    public static final int THURSDAY = 3;
    public static final int FRIDAY = 4;
    public static final int SATURDAY = 5;
    public static final int SUNDAY = 6;

    enum Weekday { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };

    // 위처럼 열거 타입을 사용하면 월요일에 접근할 때, Weekday.MONDAY 로 접근하여 사용할 수 있다.
    // Weekday 변수를 선언하고 초기화하는 방법은 아래와 같다.
    Weekday startDAY = Weekday.MONDAY;
    ```
    </blockquote>