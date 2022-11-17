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
    // 0.8999999999999999
    ```
    <blockquote>
    위의 경우, 0.9가 아닌 다른 값을 출력한다. 따라서 임의의 정밀도로 반올림 오류가 없는 정확한 숫자 계산이 필요할 때는 부동 소수점 수를 사용하지 않고, BigDecimal 클래스를 사용해야 한다.
    </blockquote><br>

    ```java
    BigDecimal.valueOf(2, 0).subtract(BigDecimal.valueOf(11, 1));
    // 0.9
    ```
    BigDecimal.valueOf(n, e)를 호출하면 값이 $n * 10^-e$ 인BigDecimal 인스턴스를 반환한다.
    <br>
    <h3>1.2.3 char 타입</h3>
    
    ---

    <blockquote>
    char 타입은 자바가 사용하는 UTF-16 문자 인코딩의 ‘코드 유닛(code unit)’을 나타낸다.

    ex) ‘J’ 는 값이 74(16진수로는 4A)인 문자 리터럴로 유니코드 문자 ‘U+004A(라틴 대문자 J)’를 나타내는 코드 유닛이다. 접두어 \u를 사용해 코드 유닛을 16진수로 나타낼 수도 있다(’\u263A’는 ‘J’).

    특수 코드 ‘\n’, ‘\r’, ‘\t’, ‘\b’ 는 각각 라인피드 (line feed), 캐리지 리턴(carriage return) - 출력 위치를 맨 앞으로 이동, 탭(tab), 백스페이스(backspace)를 나타낸다.

    ‘’ 와 \ 를 나타내기 위해서는 \로 이스케이프(escape)한다(\’ ,  \\). 
    </blockquote><br>

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
        <br>

    <h2>1.4 산술 연산</h2>

    ---

    자바는 c언어에 기반을 둔 언어로써 c에서 사용하는 연산자와 거의 비슷하다.

    <image src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F997A014D5A90B9B00D">

    위 표는 우선순위에 따라 내림차순으로 연산자를 나열한 기법이다. 

    ex) i -= j -= k 여기서 -=는 오른쪽으로 결합하므로 i -= (j -= k) 가 된다.

    
    <br>
    <h2>1.4.1 할당</h2>

    ---

    “=” 은 할당 연산자이다.

    ```java
    x = 10;
    // 이 문장은 x를 10으로 설정한다.

    x -= 5; 
    // 연산자 뒤에 =을 붙인 복합 할당 연산자는 연산자의 왼쪽과 오른쪽을 결합한 후 결과를 왼쪽에 해당한다.
    ```
    <br>
    <h3>1.4.2 기본 계산</h3>

    ---

    덧셈, 뺄셈, 곱셈, 나눗셈이 해당된다. 여기서 주의할점은 나눗셈인데, “/”로 사용하는 나눗셈은 두 피연산자가 정수 타입이면 정수 나눗셈을 의미해 나머지를 버리게 된다. 

    ```java
    result = 17.0 / 5;
    // result == 3.4

    result = 17 / 5;
    // result == 3
    ```

    또한, 모든 정수를 0으로 나누게 되면 예외가 발생한다. 이 예외를 잡지 않으면 프로그램을 종료한다. 여기서 눈여겨 볼 점은 부동소수점을 0으로 나누게 될 때인데, 이때는 프로그램을 종료하지 않고 결과로 무한대 값이나 NaN이 나온다.
    <br><br>
    % 연산자는 나머지를 돌려준다.

    ```java
    result = 17 % 5;
    // result == 2
    // 만약 a % b == 0 이면, a는 b의 정수배이다.
    ```

    만일 원하는 숫자가 0 ~ 11 사이라면, value % 12 를 통해 간단하게 얻을 수 있다. 하지만, 음수를 나누게 되면 0 ~ -11 사이의 수가 나올 수도 있다. 따라서 이럴 경우 Math.floorMod(value, 12)를 사용한다.
    <br><br>
    <blockquote>
    증가 연산자 : n++; // n에 1을 더한다.

    감소 연산자 : n—; // n에 1을 뺀다.

    n++ 와 ++n 의 차이점

    둘다 n을 1 증가시키지만, 표현식 내부에서는 서로 다른 값이 된다. n++는 증가 이전 값을, ++n은 증가 이후 값을 돌려준다. ex) String arg = args[n++] → 이는 arg를 args[n] 값으로 설정한 후, n을 1증가시킨다.
    <br><br>
    컴파일러가 코드 최적화를 잘 하지 못하던 예전에는 효율적인 방법이였지만, 현재는 두 문장으로 나누어도 수행 성능이 떨어지지 않으며, 많은 개발자들은 두 문장으로 나눈 형태가 더 읽기 쉽다고 생각한다. 
    </blockquote><br>

    <h3>1.4.3 수학 메서드</h3>
    
    ---

    ```java
    Math.pow(x, y) // x를 y번만큼 곱함(제곱 연산).
    Math.sqrt(x)   // x의 제곱수를 구함.
    Math.max(x, y) // 두 값의 최댓값을 구함.
    Math.min(x, y) // 두 값의 최솟값을 구함.

    int value = 1000000000 * 3;
    System.out.println(value);
    // -1294967296

    int value = Math.multiplyExact(1000000000, 3);
    System.out.println(value);
    // Exception in thread "main" java.lang.ArithmeticException: integer overflow

    // 이상한 값 대신 에러처리 가능.
    Math.addExact(x, y)        // 덧셈.
    Math.subtractExact(x, y)   // 뺄셈.
    Math.multiplyExact(x, y)   // 곱셈.
    Math.incrementExact(x)     // 1을 추가.
    Math.decrementExact(x)     // -1을 추가.
    Math.negateExact(x)        // 부호 변경.
    Math.toIntExact(x)         // long -> int 변경.
    // 이는 모두 int와 long 매개변수 버전을 제공한다.

    // -- example --
    int value = 1000000000 * 3;
    System.out.println(value);
    // -1294967296

    int value = Math.multiplyExact(1000000000, 3);
    System.out.println(value);
    // Exception in thread "main" java.lang.ArithmeticException: integer overflow
    ```

    위 메서드들은 정적(static) 메서드이기에 객체(instance)로 호출할 수 없다. 따라서 ‘.’연산자를 이용해 접근해야 한다.
    <br><br>
    <h3>1.4.4 숫자 타입 변환</h3>

    ---

    <blockquote>
    연산을 하고자 할 때, 피 연산자의 숫자 타입이 다르다면 연산될 피 연산자들의 숫자를 공통 타입으로 변환 후 결합한다.

    1. 둘중 하나가 double 타입이면 double로 변환.
    2. 둘중 하나가 float 타입면 float로 변환.
    3. 둘중 하난가 long 타입이면 long로 변환.
    4. 나머지는 int로 변환.

    합법적 변환 (정보 손실이 없는 경우에만 해당)

    - byte → short, int, long, double
    - short → int, long, double
    - int → long, double
    </blockquote>
    <br>
    허용되는 것을 제외한 변환을 수행할 때는 캐스트 연산자를 사용해야 한다.

    캐스트연산자 - 대상 타입의 이름을 괄호 안에 넣은 형태

    ```java
    double x = 3.75;
    int n = (int)x;
    // n == 3
    ```

    타입 변환 대신 가까운 정수로 반올림하고 싶을 때는 Math.round 메소드를 사용한다. 이 메소드의 반환값은 long타입이기 때문에, int 변수에 넣기 위해서는 (int)Math.round(x); 를 해주면 된다.

    ```java
    int n = 1;
    char next = (char)('J'+n) 
    // 1. char 타입과 int 타입이므로 둘다 int타입으로 변환되어 계산
    // 2. 계산된 75를 char타입으로 변환 == 'K'
    ```

    long 타입을 int 타입으로 캐스트 연산자를 사용해 강제로 변환하면 하위 바이트들만 보존되므로, 이 경우에는 Math.toIntExact() 함수를 사용하여 오류를 처리하는게 좋다.

    <br>
    <h3>1.4.5 관계 연산자와 논리 연산자</h3>

    ---

    ```java
    // == -> 같다.
    // != -> 다르다.
    // && (논리곱) -> 둘다 true일 때,
    // || (논리합) -> 둘중 하나라도 true일 때,
    // ! (논리부정) -> ~이 아닐때,
    // ? (조건) -> 조건 한개와, 값 두개가 존재할 때, true면 첫 번째 값이 되고, false면 두 번째 값이 된다.
    ```
    <blockquote>
    단락 평가(short circuite valuation)는 두 번째 조건이 오류를 일으킬 가능성이 있을 때 유용하다. 논리곱과 논리합이 단락 평가에 해당하며, &&(논리곱)은 첫 번째 조건이 false시 두 번째 조건은 평가하지 않고, ||(논리합)은 첫 번째 조건이 true면 두 번째 조건은 평가하지 않는다. 따라서 두 번째 조건에 오류가 나는 상황이라도 평가하지 않기에 오류를 일으킬 가능성이 있을 때 유용하다.
    </blockquote><br>
    <h3>비트 단위 연산자</h3>

    ```java
    // & (and, 비트곱) -> 둘다 1일때,
    // | (or, 비트합) -> 하나라도 1일때,
    // ^ (xor, 배타적 비트합) -> 둘 중 하나만 1일때.
    // ~ (연산자의 !에 해당) -> 인수의 모든 비트를 뒤집는다.
    ```

    <br>
    <h3>1.4.6 큰 숫자</h3>

    ---

    BigInteger, BigDecimal

    BigInteger 클래스는 임의로 정밀도 정수 연산을 구현, BigDecimal 클래스는 부동소수점 수에 동일 동작을 구현한다. 정적 메서드인 valueOf는 long을 BigInteger로 변환한다.

    ```java
    BigInteger n = BigInteger.valueOf(123456789012345678L);
    BigInteger k = new BigInteger(123456789012345678);

    // BigInteger에 미리 정의된 상수로 BigInteger.ZERO, BigInteger.ONE, BigInteger.TWO, 
    // BigInteger.TEN 등이 있다.
    // Java에서는 객체에 연산자를 사용할 수 없으므로 큰 숫자를 다룰 때는 반드시 메서드를 호출해야 한다.

    BigInteger r = BigInteger.valueOf(5).multiply(n.add(k));
    // r = 5 * (n + k)
    ```
    <br><br>

    <h2>1.5 문자열</h2>

    ---
    <blockquote>
    문자열은 문자의 연속이다. 자바에서는 유니코드 문자라면 무엇이든 문자열에 쓸 수 있다.
    </blockquote><br>
    <h3>1.5.1 문자열 연결</h3>
    
    ---

    문자열은  +로 연결할 수 있다. 

    ```java
    String args = "Hello" + " Java";
    // Hello Java
    int age = 24;
    String output = age + args;
    // 24Hello Java 

    // 자동으로 문자열로 변환되어 병합되기 때문에, 조심해야 한다.
    "My age is " + age + 1;
    // My age is 241
    "My age is " + (age + 1);
    // My age is 25

    String names = String.join(", ", "Peter", "Paul", "Mary");
    // names를 "Peter, Paul, Mary" 로 설정.
    // 첫 번째 인자로 구분자를 설정할 수 있다.

    // StringBuilder
    StringBuilder sb = new StringBuilder();
    sb.append("문상우 ").append("김우원 ").append("나청송 ");
    String result = sb.toString();
    // 여러 합쳐질 문자열을 하나로 관리 후, toString 함수를 통해 String 타입으로 처리.
    ```
    <br>
    <h3>1.5.2 부분 문자열</h3>

    ---

    ```java
    // substring()

    String location = "Hello, World!".substring(7, 12);
    // World
    // 첫번째 인자 -> 추출할 부분 문자열의 시작 위치
    // 두번째 인자 -> 추출하지 않을 부분의 시작 위치
    // 첫번째 인자부터 두번째 인자를 포함하지 않는 부분까지 추출한다.

    // split()

    String names = "문상우, 김우원, 나청송";
    String[] result = names.split(", ");
    // 문자열 세 개로 구성된 배열 ["Peter", "Paul", "Mary"]
    // split 안에 들어가는 분리자(seperator)로 어떤 정규 표현식이든 사용할 수 있다.
    ```
    <br>
    <h3>1.5.3 문자열 비교</h3>
    
    ---

    ```java
    // equals()

    location.equals("world");
    // location의 리터럴 값이 "world" 라면 true

    location.equals("world");
    // location의 리터럴 값이 대소문자가 섞인 world라면 false를 반환한다.

    // 대소문자를 구분하지 않고 두 문자열을 비교하기 위해서는 equalsIgnoreCase 메서드를 사용한다.
    loaction.equalsIgnoreCase("world");

    "first".compareTo("second");
    // 오름차순으로 한 문자열이 또 다른 문자열보다 앞에 오는지를 알려준다.
    // 앞에 온다면 음의 정수를 반환
    // 뒤에 온다면 양의 정수를 반환
    // 같다면 0을 반환한다.

    // 문자열 어떤 객체도 가능(null도 가능)
    String middleName = null;
    // null 검사 (equals도 가능)
    if (middleName == null) {
        System.out.println(middleName + "은 null입니다.");
    }
    // null은 null입니다. 
    ```

    <h3>== 이 아니라 .equals()를 쓰는 이유.</h3> 
    <blockquote>
    문자열은 객체이다. 따라서 각  객체에 해당 리터럴 값이 저장되는데, ==으로 비교하게 되면 이 객체 참조를 조사한다. 따라서 리터럴 값이 같아도 객체가 다르면 false가 나오게 된다. 하지만 equals를 쓰게 되면 다른 객체이더라도 리터럴 값만 같다면 true를 출력해주기에 단순 값 비교를 위해서는 .equals()를 써야 한다. 
    </blockquote><br><br>
    <h3>1.5.4 숫자와 문자열 사이의 변환</h3>

    ---

    Integer.toString

    정수를 문자열로 변환할때 호출됨.

    ```java
    int n = 42;
    String str = Integer.toString(n); // str을 "42"로 설정
    String str2 = Integer.toString(n, 2); // str을 "101010"로 설정

    int n = Integer.parse(str2); // n을 101010으로 설정함. (2진수 보이는 모습 그대로 변환해버림)
    int n2 = Integer.parse(str2, 2); // n을 42으로 설정함. (2진수에 해당하는 수로 변환)

    // 부동소수점
    String str = Double.toString(3.14); // str을 "3.14"로 설정한다.
    double x = Double.parseDouble(str); // x를 3.14로 설정한다.
    ```

    <br>

    <h3>1.5.5 문자열 API</h3>

    ---

    String class의 메서드들이다.

    <image src="https://t1.daumcdn.net/cfile/tistory/232F4E4955F77F890B">
    <blockquote>
    자바에서 String 클래스는 불변(immutable)이다. 해당 문자열 자체를 변경할 수는 없고 새로운 문자열을 반환하는 것이다.

    또, 일부 메서드는 CharSequence 타입 매개변수를 받는다. CharSequence 는 String, StringBuilder, 다른 문자 시퀀스의 공통 슈퍼타입(supertype)이다.
    </blockquote><br>

    <h3>1.5.6 코드  포인트와 코드 유닛</h3>
    
    ---
    <blockquote>
    자바는 유니코드 표준을 사용해 왔고, 표준화를 시키려 노력했다. 하지만 여러 국가들의 언어를 포함시킴으로써 필요로하는 비트 수가 많아졌고 이에 서로 다른 인코딩으로 된 파일을 교환하는 문제점이 발생했다.

    처음부터 16비트 코드를 할당하는 방법으로 이 문제를 해결하기 시작했지만, 문자가 예상보다 훨씬 많다는 문제점이 발생한다. 이에 현재 유니코드는 21비트를 요구하고, 유효한 유니 코드 값을 코드 포인트(code point)라고 한다. 

    ex) A 문자의 코드 포인트는 U+0041

    UTF-16은 길이가 가변적이고 하위 호환성을 지원하는 인코딩이다. 이 인코딩은 모든 유니코드 문자를 16비트 값 한 개로 나타내고, U+FFFF를 넘는 문자는 “대용 문자”라는 코드 공간의 특별한 영역에서 가져온 16비트 값의 쌍으로 나타낸다. 

    자바 문자열은 유니코드 문자(코드 포인트)의 원래 시퀀스가 아닌 UTF-16으로 인코딩된 16비트 숫자인 코드 유닛의 시퀀스다.

    특수 문자 또는 중국어 표의 문자를 버릴 것이라면 다음처럼 코딩한다.

    ```java
    char ch = str.charAt(i);
    int length = str.length();
    ```

    하지만, 문자열을 제대로 처리하려면 더 많은 작업이 필요하다. i번째 유니코드의 코드 포인트를 얻기 위해서는 다음과 같이 호출한다.

    ```java
    int codePoint = str.codePointAt(str.offsetByCodePoints(0, i));

    // 코드 포인트의 총수
    int length = str.codePointCount(0, str.length());

    // 이 루프는 코드 포인트를 순차적으로 추춘한다.
    int i = 0;
    while (i < s.length) {
        int j = s.offsetByCodePoints(i, 1);
        String codePoint = str.substring(i, j);
        // 내용 생략
        i = ;
    }
    ```
    </blockquote>
        <br><br>

    <h2>1.6 입력과 출력</h2>

    ---

    <h3>1.6.1 입력 읽어 오기</h3>

    ---

    출력은 System.out.println을 호출하면 ‘표준 출력 스트림(standard output stream)’으로 전달되어 터미널에 표시된다. 하지만 입력은 [System.in](http://System.in) 객체에는 바이트 한 개를 읽어 오는 메소드만 존재하기에 ‘표준 입력 스트림(standard input stream)’에서 읽기는 쉽지 않다 따라서 System.in에 연결된 Scanner를 생성한다. 

    ```java
    // Scanner 클래스는 java.util 패키지에 있으므로 import를 해서 불러와야한다.
    import java.util.Scanner;

    Scanner sc = new Scanner(System.in);
    // Scanner 선언

    //nextLine 메서드는 입력을 한 줄 읽는다.
    String name = sc.nextLine();
    // 위는 입력이 공백을 포함할 수 있다.
    String firstName = sc.next();
    // 위는 공백으로 구분된 단어 한 개를 읽는다.
    int age = sc.nextInt();
    // 정수를 읽기 위해선 nextInt를 사용한다.
    // nextDouble 메서드는 다음 번 부동소수점 수를 읽는다.
    double height = sc.nextDouble();
    // 다른 줄, 단어, 정수, 부동소수점 수가 있는지 검사하고 싶을 때는
    // hasNextLine, hasNext, hasNextInt, hasNextDouble 메서드를 사용한다.
    ```
    <br>
    <h3>비밀번호를 받을 때, 사용하는 Console 클래스</h3>

    ```java
    Console terminal = System.console();
    String username = terminal.readLine("User name: ");
    char[] passwd = terminal.readPassword("Password: ");
    // readPassword로 입력을 읽으면 비밀번호가 문자 배열로 반환된다.
    // 문자 배열을 비밀번호를 사용한 후, 덮어쓸 수 있어 String 보다 조금 더 안전하다.
    ```
    <br>
    <h3>1.6.2 포맷 적용 출력</h3>

    ---

    ```java
    System.out.print("_");    // 출력 후 다음 줄이 아닌 바로 뒤에 커서가 존재.
    System.out.println("_");  // 출력 후 다음 줄에 커서 존재.
    // 위 두 명령어는 소수점을 있는 그대로 모두 출력한다.

    // 출력되는 숫자의 개수를 제한하려면 printf 메서드를 사용한다.

    System.out.printf("%8.2f", 1000.0 / 3.0);
    // 333.33
    // 포멧 문자열(format string) "%8.2f"는 부동소수점 수를 필드 폭(field width)은 8자리로 출력,
    // 정밀도(precision)는 2자리로 출력한다.
    // 이는 소수점을 2개까지 표현하고, 전체 문자 수를 8개 출력한다는 뜻이다(위 코드는 앞에 공백 두개 포함).
    // printf 에 매개변수를 여러 개 지정할 수 있다.
    ```

    포맷 지정자

    포맷 지정자는 각각에 대응하는 인수로 교체된다. 포맷 지정자 끝에 오는 변환 문자(conversion character)는 포맷이 적용될 값의 타입을 나타낸다.

    <image src="https://t1.daumcdn.net/cfile/tistory/9973C5405BE279162F">

    ```java
    System.out.printf("%,+.2f", 100000.0 / 3.0);
    // +33,333.33
    // , 는 그룹 분리자를 추가하고 +는 숫자에 부호를 붙인다.

    // String.format 메서드를 사용하면 포맷 적용 문자열을 출력하지 않고 만들 수 있다.
    String message = String.format("Hello, %s. your age is %d", "moon", 24);
    ```

        <br>
    <h2>1.7 제어 흐름</h2>

    ---

    <h3>1.7.1 분기</h3>

    ---

    if 문 (if - else if - else 가 각각 하나의 분기)

    ```java
    if (count > 0) {
        double average = sum / count;
    } else if (count == 0) {
        System.out.println(0);
    } else {
        System.out.println("Huh?");
    }
    ```

    switch 문 (제한된 상수값을 기준으로 표현식을 검사할 때 사용)

    ```java
    switch (count) {
        case 0:
            output = "None";
            break;
        case 1:
            output = "one";
            break;
        case 2:
        case 3:
        case 4:
        default:
            output = "Many";
            break;
    }
    // 일치하는 case 레이블 실행, 일치하는 레이블이 없다면 default 레이블 실행.
    // break를 빠뜨리면 다음 문장을 실행하기 때문에, 반드시 break를 써줘야 함.
    ```
    <blockquote>
    case 레이블은 정수 뿐만 아니라 char, byte, short, int 타입(이들의 래퍼런스 클래스인 Character, Byte, Short, Integer) 상수표현식, 문자열 리터럴, 열거 값을 넣을 수 있다.
    </blockquote><br><br>

    <h3>1.7.2 루프</h3>

    ---

    while 루프는 조건 검사 결과에 따라 수행할 작업이 남아 있는 동안 바디를 계속 실행한다. 
    <br>for 루프는 반복 횟수가 고정되어 있을 때 사용한다.

    ```java
    // while
    Random rand = new Random();
    while (sum < target) {
        int next = rand.nextInt(10);  // 0~9 사이의 임의의 정수
        sum += next;
        count++;
    }
    // 위는 합계(sum)가 목표 값 이상이 될 때까지 루프를 진행한다.

    // 조건을 평가하기 전에 루프를 먼저 실행해야 할때는 아래 문법을 사용한다.
    do {
        next = rand.nextInt(10);
        count++;
    } while (next != target);
    // 문장을 한번 들어가 next와 count에 대한 연산을 끝내고 처음으로 조건을 평가한다.

    // for
    for (int i = 1; i <= 20; i++) {
        int next = rand.nextInt(10);
        sum += next;
    }
    // 위는 i를 20번 반복하고 0~9의 임의의 수를 sum에 더한다.
    int i = 1;
    while (i <= 20) {
        int next = rand.nextInt(10);
        sum += next;
        i++;
    }
    // 위는 위에 있는 for문을 while문으로 바꾼 것이다.
    // 이렇게 되면 i 변수의 초기화, 검사, 업데이트가 각기 다른 위치에서 하게 되므로 for문을 사용해야 한다.

    // for 루프의 헤더(선언부)에서 변수를 선언하는 대신 기존 변수를 초기화해도 된다.
    for (i = 1; i <= target; i++) // 기존 i변수 사용.

    // 콤마를 사용해 여러 변수를 선언하거나 초기화하고 업데이트 할 수 있다.
    for (int i = 0, j = n-1; i < j; i++, j--) 

    // 초기화나 업데이트가 필요 없다면 비워둘 수 있다. 조건을 생략하면 항상 true로 간주된다.
    for (;;) // 무한 루프 
    ```
    <br>

    <h3>1.7.3 중단과 계속</h3>

    ---
    <br>
    break

    ```java
    boolean done = false;
    String input;
    while (!done) {
        input = sc.next();
        if ("Q".equals(input)) {
            done = true;
        } else {
            System.out.println("Q를 입력하면 종료.");
        }
    }
    // 위는 boolean 변수로 루프를 제어해서 중간에 루프를 빠져나오는 방법이다.

    String input;
    while (true) {
        input = sc.next();
        if ("Q".equals(input)) break;         // 루프 종료
        System.out.println("Q를 입력하면 종료.");
    }
    // break시에 이곳으로 넘어옴.
    // 위는 break를 사용하는 방법이다.
    ```
    <br>
    continue

    ```java
    while (sc.hasNextInt()) {
        int input = sc.nextInt();
        if (n < 0) continue;     
        // 검사 부분 즉 sc.hasNextInt()으로 건너뛰고 밑의 코드는 실행하지 않는다.
    }

    // for 루프 안에서 사용하면 continue 문은 그 다음 업데이트 문장으로 건너뛰게 한다.
    for (int i = 1; i <= target; i++) {
        int input = sc.nextInt();
        if (n < 0) continue; // i++로 건너뛰고 밑의 코드는 실행하지 않는다.
    }
    ```
    <br>

    <h3>Labeled Break</h3>

    break 문은 자신을 감싼 루트 한개만 빠져나온다. 따라서 중복 반복문을 탈출하기 위해서는 레이블을 붙인 break 문을 사용해야 한다.

    ```java
    sangwoo:
    while (...) {
        while (...) {
            while (...) {
                if (...) break sangwoo;
            }
        }
    }
    // 레이블이 붙어 있는 while문을 탈출한다.
    // 레이블은 문장의 위쪽에 붙이지만, break 문은 해당 문장의 끝으로 건너뛴다.

    // 이 레이블을 붙인 break는 블록 문을 포함해서 어떤 문장의 끝으로든 제어를 이동할 수 있다.
    sangwoo: {
        if (...) break sangwoo;
    }
    // 레이블을 붙인 break는 이 위치로 건너뛴다.

    // continue도 동일 coutinue는 해당 루프의 다음 조건문, 다음 업데이트 문장으로 이동한다.
    ```
    <br><br>

    <h3>1.7.4 지역 변수의 유효 범위</h3>

    ---
    <blockquote>

    스코프(scope)
    <blockquote>
    스코프란, 변수의 유효 범위로써, 프로그램에서 해당 변수에 접근할 수 있는 범위이다.
    </blockquote>
    <br>

    지역 변수(local variable)
    <blockquote>
    지역 변수는 메서드의 매개변수를 포함해 메서드 안에 선언한 변수이다. 지역 변수의 스코프는 변수 선언 지점에서 시작해 해당 선언을 감싼 블록의 끝까지 이어진다. 매개 변수의 유효 범위는 메서드 전체이다.
    </blockquote>
    <br>
    </blockquote>
    <br>

    ```java
    {
        int age = 24;
        ...
        // 여기서 age의 스코프는 끝.
    }
    ```
    <br><br>

    루프가 반복될 때마다, 변수의 사본을 새로 만들고 루프 바깥쪽에는 이 변수가 존재하지 않는다.
    

    ```java
    int next;
    do {
        next = rand.nextInt(10);
        count++;
    } while (next != target);
    // 만일 위에서 next를 루프 안쪽에서 선언했다면, next의 유효 범위가 루프 바디의 끝까지만 이어졌을 것이다.

    // for 루프에서 변수를 선언하면 해당 변수의 스코프는 검사와 업데이트 부분을 포함해 루프의 끝까지 이어진다.
    for (int i = 0; i < n; i++) { // i가 검사 및 업데이트의 유효 범위 안에 있다.
        ...
    }
    // 이곳은 i가 정의되어 있지 않은 곳.

    // 루프가 끝난 후에도 i를 사용하고 싶다면 i를 변수 밖에서 선언해야 한다.
    int i = 0;
    for (int i = 0; i < n; i++) {
        ...
    }
    // 이곳에서도 i를 사용할 수 있다.

    // 따라서 java에서는 서로 겹치는 유효 범위에 이름이 같은 지역 변수를 여러개 둘 수 없고, 
    // 유효 범위가 겹치지 않는다면 변수 이름이 같아도 재사용할 수 있다.

    int i = 0;
    while (...) {
            String i = sc.next(); // 또 다른 변수를 선언하는 것은 오류.
        ...
    }

    for (int i = 0; i < n; i++) { ... }
    for (int i = n/2; i < n; i++) { ... } // i를 재정의해도 상관없다.
    ```

    <br><br>
    <h2>1.8 배열과 배열 리스트</h2>

    ---

    <h3>1.8.1 배열 다루기</h3>
    
    ---

    모든 타입에는 대응하는 배열 타입이 존재한다. 정수 → int[], 문자열 → String[] 등.

    ```java
    // 배열 선언 및 사용
    String[] name; // String name[] 도 가능하지만 거의 사용하지 않음.
    name = new String[100];
    // ==
    String name = new String[100];

    // name은 100개의 String타입으로 된 요소를 가지고 인덱스의 범위는 0~99이다.
    // 만일 인덱스 -1또는 100에 접근하게 된다면, ArrayIndexOutOfBoundsException 오류가 발생한다.

    name.length // 배열의 길이를 알 수 있다 (array.length)
    ```
    <br>
    <h3>1.8.2 배열 생성</h3>

    ---

    new 연산자로 배열을 생성할때, 기본적으로

    - 숫자 타입(char포함)의 배열은 0으로 채운다.
    - boolean의 배열은 false로 채운다.
    - 객체의 배열은 null 참조로 채운다.
    
    <br>
    객체의 배열을 생성한 후에는 객체로 채워야 한다(아무것도 초기화 된 것이 없고 null을 참조할 뿐이기 때문에)

    ```java
    BigInteger[] numbers = new BigInteger[100];

    for (int i = 0; i < 100; i++) 
    numbers[i] = BigInteger.valueOf(i)
    // 해당 인덱스에 해당하는 요소에 인덱스 값을 넣어주는 코드이다(초기화).

    // 반복문 없이 배열을 초기화 하는 법.
    int[] primes = { 2, 3, 4, 5, 6, 10, 12, };
    // 이 때는, new 연산자를 사용하지 않고, 배열의 길이도 지정하지 않는다. 
    // 마지막 요소 뒤에는 쉼표를 넣어도 무관하다. 배열에 값을 추가해 나갈 때 편리하다.

    // 기존에 존재하는 배열에 새 배열을 할당하는 법.
    primes = new int[] { 17, 19, 23, 29, 31 };
    ```
    <blockquote>
    Note. 길이가 0인 배열을 만들 수 있다. 생성은 new int[0], new int[] {} 로 생성. 일치하는 항복의 배열을 반환하는 메서드는 특정 입력과 일치하는 항목이 없을 때, 길이가 0인 배열을 반환한다. 이를 이용하는 방법은 a가 길이가 0인 배열이라면, a.length == 0으로 검사를 할 수 있지만, a가 null이라면 a.length는 NullPointerException을 일으킴으로 이를 이용해 코드를 작성할 수 있다.
    </blockquote><br>

    <h3>1.8.3 배열 리스트</h3>

    ---

    ArrayList
    <blockquote>
    배열은 미리 길이를 알아야 하고, 한번 생성하면 절대로 길이를 변경할 수 없다. 이러한 단점을 보완하기 위해 나온 자료구조가 ArrayList이다. 이 객체는 내부에서 배열을 관리하고, 개발자에게는 보이지 않는다.<br><br>

    ArrayList는 제레닉 클래스(generic class), 즉 타입 매개변수가 있는 클래스이다. 제네릭은 <>를 통해 타입을 지정한다.
    </blockquote><br>

    ```java
    ArrayList<String> friends;
    friend = new ArrayList<>();
    // ==
    firend = new ArrayLi<String>();
    ```

    컴파일러는 변수의 타입에서 타입 매개변수를 추론한다. 
    위 호출에서 생성 인수가 없지만, 그럼에도 끝내 ()를 붙여야 한다. 결과는 크기가 0인 배열 리스트.<br><br>

    ```java
    ArrayList<String> friends = new ArrayList<>(List.of("Peter", "Paul"))
    // List.of 메서드는 지정한 요소들로 구성된 수정 불가능한 리스트를 반환한다.
    // 배열 리스트용 초깃값 지정 문자가 없기에 위처럼 초기화 해주어야 한다.

    friends.add(1, "moon");    // 인덱스 1자리에 moon이 추가되고 그 뒤의 배열들은 밀림.
    friends.remove(2);         // 2인덱스 삭제.
    firstName = firends.get(1) // 1번 인덱스 값 불러옴.
    firends.set(1, "song")     // 1번 인덱스 값 moon -> song 으로 변경.
    firends.size()             // 리스트의 현재 크기를 돌려줌.
    ```

    <br>
    <h3>1.8.4 기본 타입의 래퍼 클래스</h3>
    
    ---

    제네릭 클래스는 기본 타입을 타입 매개변수로 사용할 수 없다는 불편한 제약이 존재한다. 따라서 이 해결책으로 래프 클래스(wrapper class)를 사용한다. 래퍼 클래스 (Byte, Short, Integer, Long, Float, Double, Character, Boolean)는 등이 존재한다.

    ```java
    ArrayList<Integer> numbers = new ArrayList<>();
    numbers.add(42);
    int first = numbers.get(0);
    // 정수형 배열 리스트 생성 -> 배열 리스트 에 42 추가 -> first에 정수형 숫자 
    ```

    기본 타입과 래퍼타의 변환은 자동으로 일어난다.

    1. add를 호출할 때, 오토박싱(autoboxing)을 거쳐 42를 담은 Integer 객체를 생성
    2. int형 변수 first에 넣기 전에 Integer 내부의 int 값을 돌려주도록 언박싱(unboxing)된다.
    <br><br>
    <blockquote>
    Caution

    연결 리스트 내부에 존재하는 값은 래퍼 타입의 객체이므로 이 객체끼리 비교할 때는 equals를 사용한다.
    </blockquote><br>
    <h3>1.8.5 향상된 for 루프</h3>

    ---

    ```java
    int sum = 0;
    for (int i = 0; i < numbers.length; i++) {
        sum += numbers[i];
    }
    // 위는 numbers 의 길이만큼 그 내부 값을 탐색한다.

    int sum = 0;
    for (int n : numbers) {
        sum += n;
    }
    // 위 향샹된 루프는 배열의 인덱스 값이 아닌 요소를 순회한다(배열 리스트도 사용 가능).
    ```

    <br>

    <h3>1.8.6 배열과 배열 리스트 복사</h3>

    ---

    배열 변수를 다른 배열 변수로 복사하게 되면, 두 변수는 같은 배열을 참조하게 된다. 이렇게 되면 하나의 배열 변수를 고쳐도 뜻하지 않은 다른 배열 변수도 함께 수정되거나 삭제될 수 있다. 따라서 배열의 사본을 만들어 복사해야 한다.

    ```java
    int[] copiedPrimes = Arrays.copyOf(primes, primes.length);
    // 위는 새 배열을 원하는 길이로 생성, 원본 배열의 요소를 복사한다.
    ```

    배열 리스트를 복사하기 위해서는 기존 배열 리스트에서 새 배열 리스트를 생성해야 한다. 

    ```java
    ArrayList<String> friends = new ArrayList<>(List.of("Peter", "Paul"));
    ArrayList<String> people = friends;
    people.set(0, "Mary"); // friends.get(0)의 값도 "Mary"가 됨.

    ArrayList<String> copiedFriends = new ArrayList<>(friends);
    copiedFriends.set(0, "moon");
    System.out.println(friends);            // [Peter, Paul]
    System.out.println(copiedFriends);      // [moon, Paul]
    // 위 코드는 같은 곳 참조 x.

    String[] names = { "moon", "na", "kim", "won" };
    ArrayList<String> friends = new ArrayList<>(List.of(names));
    System.out.println(friends);
    // [ "moon", "na", "kim", "won" ]
    // 배열을 배열 리스트에 복사할 때는 위처럼 Lise.of 메서드를 사용해 생성하면 된다.

    String[] names1 = friends.toArray(new String[0]);
    for (String n : names1) {
        System.out.printf("%s ", n);
    }
    // moon na kim won
    // 배열 리스트를 배열에 복사한다. 하위 호환성 때문에 반드시 올바른 타입으로 된 배열을 전달해야 한다.
    ```

    <br>

    <h3>1.8.7 배열 알고리즘</h3>
    
    ---

    - Arrays 와 Collections 클래스는 배열과 배열 리스트에서 자주 사용하는 알고리즘을 구현한다.
    - Array.fill → 배열을 채운다.
    - Collections.fill → 배열 리스트를 채운다.
    - Arrays.sort → 배열을 정렬한다. (parallelSort 는 대상이 클 때 사용하고, 배열 리스트는 사용할 수 없다.)
    - Collections.sort → 배열 리스트를 정렬한다.
    - Collections.reverse → 요소들을 뒤집는다.
    - Collections.shuffle → 요소들을 임의로 섞는다.
    
    <br>

    Arrays.toString 메서드는 배열을 문자열로 표현한 결과를 돌려준다.

    ```java
    System.out.println(Arrays.toString(primes));
    // [2, 3, 4, 5, 6, 10, 12] 을 출력.

    String elements = friends.toString();
    // elements 를 "[Peter, Paul]" 로 설정.

    // 출력할 때는 println 메서드가 알아서 toString을 처리한다.
    ```

    <br>

    <h3>1.8.8 명령줄 인수</h3>

    ---

    Java program의 main method는 문자열의 배열을 매개변수로 받는다. 프로그램을 실행하면 매개변수가 명령줄 (command line)에서 지정한 인수들로 설정된다.

    <br>

    <h3>1.8.9 다차원 배열</h3>

    ---

    자바는 배열의 배열로 다차원 배열을 구현한다. 각 행의 요소의 개수가 같지 않을 수 있다. 단지 객체에서 다른 객체를 참조하는 것 뿐.

    ```java
    int[][] arr_2 = {
        { 1, 2, 3, 4 },
        { 5, 6, 7 }
    };
    // arr_2[0] 은 { 1, 2, 3, 4 } 를 담고 있는 객체를 참조하고 있다.
    // arr_2[1] 은 { 5, 6, 7 } 를 담고 있는 객체를 참조하고 있다.
    // arr_2[0][2]는 3이라는 변수에 접근 가능하다.

    int[] temp = arr_2[0];
    arr_2[0] = arr_@[1];
    arr_2[1] = temp; 
    // 이렇게 행을 맞바꿀 수도 있다.

    // 만일 초기값을 제공하지 않을 때는 반드시 new연산자를 사용하고 행과 열의 개수를 지정해야 한다.
    int[][] arr_ = new int[4][4];
    int[][] arr_3 = new int[4][];
    // 위 둘다 가능하다.

    int[][] arr = new int[4][4];
    arr[1] = new int[]{1, 2, 3};
    arr[2] = {1, 2, 3}; // 오류 발생. new int[] 를 사용하여 객체를 생성해주어야 한다.

    // 순환
    for (int i = 0; i < arr_2.length; i++){
        for (int j = 0; j < arr_2[i].length; j++) {
            System.out.printf("%d ", arr_2[i][j]);
        }
        System.out.println();
    }
    for (int[] row : arr_2) {
        for (int element : row) {
            System.out.printf("%d ", element);
        }
        System.out.println();
    }
    // 위 둘다 한번씩 순환한다.
    ```

    <br>

    <h2>1.9 기능적 분해</h2>

    ---

    <h3>1.9.1 정적 메서드 선언 및 호출</h3>

    ---

    ```java
    public static double average (double x, double y) {
        return x + y;
    }
    // 메서드 헤더(메서드 선언부)
    // 반환 값의 타입, 메서드 이름, 매개변수의 타입과 이름

    // 메서드 바디(메서드 구현부)
    // 구현할 내용을 채워 넣고, 반환할 결과는 return을 사용한다.
    ```

    <br>

    <h3>1.9.2 배열 배개변수와 반환 값</h3>

    ---

    배열을 메서드에 전달할 수 있다. 메서드는 배열에 대한 참조를 받으며, 이 참조로 전달받은 배열을 변경할 수 있다.

    ```java
    public static void swap(int[] values, int i, int j) {
        int temp = values[i];
        values[i] = values[j];
        values[j] = temp;
    }
    // 위는 두 요소를 맞바꾸는 메서드이고 다른 반환값없이 참조하는 배열의 내부 요소를 바꿀 수 있다.

    public static int[] fristLast(int[] values) {
        if (values.length == 0) return new int[0];
        else return new int[] {values[0], values[values.length - 1]};
    }
    // 메서드는 배열을 반환할 수 있다.
    ```

    <br>

    <h3>1.9.3 가변 인수</h3>

    ---

    호출하는 쪽에서 인수를 원하는 개수만큼 넘길 수 있게 하는 메서드이다. ex) printf

    <br>

    임의의 개수의 수를 받아 평균을 구하는 코드

    ```java
    public static double average(double ... values) {
        double sum = 0;
        for (double v : values) sum += v;
        return values.length == 0 ? 0 :sum / values.length;
    }
    // 타입 뒤에 ..을 붙여서 '가변 인수(varargs)' 매개변수를 선언한다.
    // values 매개변수는 double 타입 배열이 된다. 메서드를 호출할 때 배열이 생성되고,
    // 호출 하는 쪽에서 전달한 인수들로 채워진다. 배열을 인수로 전달 가능하다.

    // 다른 매개변수를 가변 매개변수 앞에 둘 수 있다.
    // 가변 매개변수는 반드시 매개변수의 마지막 매개변수여야 한다.
    public static double max(double first, double ... rest) {
        double result = first;
        for (double v : rest) result = Math.max(v, result);
        return result;
    }
    // 첫번째 들어오는 인수를 기본 값으로 설정후,
    // 뒤이어 들어오는 인수들을 기본 값과 비교.
    ```
