<blockquote>

**코드를 작성할 때, 정상적인 상황보다는 예상하지 못한 상황을 다룰 때, 코드가 더 길고 복잡하기 마련이다. 자바는 다른 현대 프로그래밍 언어와 마찬가지로 실패 지점에서 제어를 관련 핸들러(handler)로 넘기는 견고한 예외 처리 메커니즘을 제공한다. 이외에도 자바의 assert문을 통해 내부적으로 가정한 상황을 구조적이고 효율적으로 처리하고, 로깅 API를 통해 프로그램 실행 중에 일어나는 다양한 이벤트를 기록할 수 있다.**

</blockquote>

<br>

<h2>5장 핵심내용</h2>

---

- 프로그램에서 예외를 던지면 해당 예외와 가장 가까운 예외 핸들러로 제어가 전달된다.
- 자바에서는 컴파일러가 검사 예외를 추적한다.
- 예외를 처리하려면 try/catch 구조를 사용해야 한다.
- try-with-resources 문은 정상적으로 실행을 마치거나 예외가 일어나면 자동으로 리소스를 닫는다.
- 실행이 정상적으로 진행되든 그렇지 않든 반드시 해야 할 일이 있을 때는 try/finally 구조를 사용해야 한다.
- 예외를 잡아서 다시 던지거나 다른 예외와 연쇄할 수 있다.
- 스택 추적은 실행의 특정 지점에서 대기 중인 모든 메소드 호출을 보여 준다.
- 클래스에 단정 검사를 활성화하면, 단정은 조건을 검사해 해당 조건을 만족하지 못하면 오류를 일으킨다.
- 로거는 계층 구조로 되어 있으며 SEVERE ~ FINEST 레벨로 된 로깅 메시지를 수신할 수 있다.
- 로그 핸들러는 로깅 메시지를 다양한 목적지로 보낼 수 있고, 포매터는 메시지 포맷을 제어한다.
- 로그 구성 파일로 로깅 프로퍼티를 제어할 수 있다.

<br><br>
    
<h2>5.1 예외 처리</h2>

---
    
자바는 예외 처리(exception handling)를 지원하므로, 메서드에서 예외를 ‘던지는’ 방법으로 심각한 문제를 알릴 수 있다. 예외 처리는 오류를 감지하는 과정과 처리하는 과정을 분리할 수 있다는 장점이 있다.

<br>
    
<h3>5.1.1 예외 던지기</h3>

---
    
메서드가 해야 할 일을 할 수 없는 상황에 빠질 수도 있다. 이때는 예외를 던지는 것이 최선이다.

<br>
    
예시로 두 경곗값 사이에 있는 임의의 정수를 돌려주는 메서드를 구현한다고 가정한다.

<br>
    
```java
public static int randInt(int low, int high) {
	return low + (int) (Math.random() * (high - low + 1);
}
```
    
알고리즘 특성상 low가 high 보다 작아야 하지만, 큰 값이 들어온다면 적절한 예외를 던지면 된다.

<br>
    
```java
if (low > high) 
	throw new IllegalArgumentException(
		String.format("low should be <= high but low is %d and high is %d", low, high));
```
    
위에서 쓰인 throw 문으로 예외를 처리할 수 있다. 위 코드에서는 예외 클래스 IllegalArgumentException의 객체를 ‘던진다’. 또 디버깅 메시지를 생성 인수로 전달해서 예외 객체를 생성했다.

<br>
    
throw문이 실행되면 정상적인 실행 흐름이 즉시 중단된다. 위는 randInt 메서드가 실행을 멈추고, 호출하는 쪽에 값을 반환하지 않는다. 제어는 예외 핸들러로 전달된다.

<br><br>
    
<h3>5.1.2 예외 계층</h3>

---
    
모든 예외는 Throwable 클래스의 서브 클래스이다. Error의 서브클래스는 예외 상황이 일어날 때, 던지는 예외이다.

<br>
    
프로그래머가 보고하는 예외는 Exception 클래스의 서브클래스이다. 이는 두가지로 나뉠 수 있다.
    
- **비검사 예외(unchecked exception)** 는 **RuntimeException**의 서브클래스다.
- 다른 예외는 모두 **검사 예외(checked exception)** 다.

<br>

프로그래머는 반드시 검사 예외를 잡아내거나 메서드 헤더에 선언해야 한다. 컴파일러는 검사 예외를 적절히 처리했는지 검사한다.

<br>

**Note**

<blockquote>

RuntimeException이라는 이름은 적절하지 않다. 모든 예외는 당연히 실행 시간(runtime)에 일어난다. 다만, RuntimeException의 서브클래스로 만든 예외는 컴파일 과정에서 검사를 받지 않는다.

</blockquote>

<br>

**검사 예외**는 실패를 예상하는 상황에서 사용하는데 주로 입력과 출력에서 많이 일어난다. 많은 예외 클래스가 IOException을 확장하기에 적합한 클래스를 사용하여야 한다. 예) 파일이 없을 땐 FileNotFoundException.

**비검사 예외**는 피할 수 없는 외부 위험 요소가 아니라 프로그래머가 만든 논리 오류를 나타낸다. NullPointerException은 검사되지 않고 모든 메소드가 던질 위험이 있기에 null 참조를 따라가지 않도록 작성해야 한다.

<br>

구현자가 검사 예외를 던질지, 비검사 예외를 던질지 결정해야 할 때가 있다.

Integer.parseInt(str) 호출은 str이 유효한 정수를 담고 있지 않으면 비검사 예외인 NumberFormatException 예외를 던지지만, Class.forName(str)은 str이 유효한 클래스 이름을 담고 있지 않으면 검사 예외인 ClassNotFoundException을 던진다.

<br>

이는 클래스의 특성 때문인데 클래스는 실제로 로드하기 전엔 그 클래스를 로드할 수 있는지 알 수 없기 때문이다.

<br>

자바 API에서 제공하는 많은 예외 클래스가 있기에 상황에 맞게 이 예외 클래스를 사용하면 되지만, 목적에 맞는 표준 예외 클래스가 없다면 Exception이나 RuntimeException, 기타 기존 예외 클래스를 확장해서 직접 만들어야 한다.

<br>

예외 클래스를 직접 만들어야 한다면, 인수가 없는 생성자와 메시지 문자열을 받는 생성자를 함께 구현하는 것이 좋다.

```java
public class FileFormatException extends IOException {
	public FileFormatException() {}
	public FileFormatException(String message) {
		super(message);
	}
}
```

<br><br>

<h3>5.1.3 검사 예외 선언</h3>

---

검사 예외를 일으킬 수 있는 메서드는 메서드 헤더의 throws 절에 해당 예외를 선언해야 한다.

<br>

```java
public void write(Object obj, String filename) 
	throws IOException, ReflectiveOperationException
```

위의 예시처럼 throws 문을 사용할 때는, 해당 메서드가 던질 수 있는 모든 예외를 나열해야 한다. 만약 이때 어떤 하나의 예외의 여러 서브클래스만을 던질 수 있다면 이 서브 클래스를 묶을 수 있는 하나의 예외만 작성해도 되지만, 관련 없는 예외를 던질 수도 있다면, 하나의 예외로 묶어서는 안된다.

<br>

**Tip**

<blockquote>

예외를 던지기보다 처리할 수 있다는 생각은 잘못되었다. 각 예외가 적합한 예외 핸들러에 전달할 수 있어야 한다.

</blockquote>

<br>

메서드를 오버라이드 할 때, 슈퍼클래스 메서드에서 선언한 예외보다 광범위한 검사 예외는 던질 수 없다. 즉, 오버라이드하는 메서드에서 그보다 범위가 좁은 예외만 던질 수 있다.

<br>

```java
@throws NullPointerException filename이 null일 때
@throws FileNotFountException filename으로 지정한 파일이 없을 때
```

위처럼 메서드에서 검사 예외나 비검사 예외를 던진다면 자바독의 @throws 태그로 문서화할 수 있다. 프로그래머 대부분은 문서화할 만한 내용이 있을 때만 이 태그로 문서화한다. 

<br>

**Note**

람다 표현식의 예외 타입은 절대로 명시하지 않는다. 하지만 람다 표현식에서 검사 예외를 던질 수 있다면 그 예외를 선언한 함수형 인터페이스에만 전달할 수 있다. 

<br><br>

<h3>5.1.4 예외 잡기</h3>

---

예외를 잡기 위해서는 try 블록을 사용해야 한다. 

```java
try {
	statements
} catch (ExceptionClass ex) {
	handler
}
```

<br>

try 블록에 들어 있는 문장(statements)을 실행하다가 주어진 예외 클래스(ExceptionClass)의 예외가 일어나면 제어가 핸들러(handler)로 이동한다. 예외 변수(ex)는 예외 객체를 참조하며, 핸들러는 필요하면 해당 예외 객체를 조사할 수 있다.

<br>

위 기본 구조를 두 가지로 바꿀 수 있다.

1. **서로 다른 예외 클래스에 대응하는 핸들러를 여러 개 두는 방법.**
    
    ```java
    try {
    	statements
    } catch (ExceptionClass ex) {
    	handler
    } catch (ExceptionClass ex) {
    	handler
    } catch (ExceptionClass ex) {
    	handler
    }
    // 위 ExceptionClass와 handler는 각각 다르다고 가정.
    ```
    
    이는 위에 있는 catch 절부터 일치하는 예외 타입을 찾고, 없으면 아래로 내려온다. 즉, 가장 상세한 예외 클래스부터 처리해야 한다.

    <br>
    
2. **핸들러 하나로 여러 예외 클래스를 처리하는 방법.**
    
    ```java
    try {
    	statements
    } catch (ExceptionClass | ExceptionClass | ExceptionClass ex) {
    	handler
    }
    // 위 ExceptionClass는 각각 다르다고 가정.
    ```
    
    위 경우 핸들러는 나열된 예외 클래스에 공통으로 있는 메서드만 예외 변수 ex로 호출할 수 있다.
    

    <br><br>

<h3>5.1.5 try-with-resources 문</h3>

---

예외 처리가 필요한 문제 중 하나는 리소스 관리이다. 만일 파일을 작성한다고 치면, 모두 작성한 후 .close() 문을 실행해야 파일을 닫을 수 있는데, 실행하는 도중 예외를 던져 close()가 호출되지 않을 수 있기 때문이다. 이게 반복돼 예외가 여러번 일어나면 시스템이 파일 핸들(file handle)을 소진할 수도 있다.

<br>

위 문제는 try 문 헤더에 리소스(resource)를 지정하여 해결할 수 있다. 리소스는 반드시 AutoCloseable 인터페이스를 구현하는 클래스에 속해야 한다.

<br>

```java
ArrayList<String> lines = ...;
try(PrintWriter out = new PrintWriter("output.txt")) { // 변수 선언
	for (String line : lines)
		out.println(line.toLowerCase());
}
// 또는 이전에 선언된 사실상 최종 변수를 헤더에 넣어도 된다.

ArrayList<String> lines = ...;
PrintWriter out = new PrintWriter("output.txt")
try(out) { // 사실상 최종 변수
	for (String line : lines)
		out.println(line.toLowerCase());
}
```

<br>

AutoCloseable 인터페이스에는 close 메서드 하나만 선언되어 있다.

```java
public void close() throws Exception
```

<br>

**Note**

<blockquote>

Closeable이라는 인터페이스도 있다. 이는 AutoCloseable의 서브인터페이스로 close 메서드 하나만 선언되어 있지만, 이 close는 IOException을 던진다.

</blockquote>

<br>

위에서 사용한 방법으로 코드를 작성하면, 정상적으로 try 블록의 끝에 이르렀든 예외가 일어났던 간에 try 블록이 끝날 때 리소스 객체의 close 메서드가 호출된다.

<br><br>

<h3>5.1.6 finally 절</h3>

---

try-with-resource 문은 예외 발생 여부와 상관없이 자동으로 리소스를 닫는 것을 알아보았다. 때로는 AutoCloseable이 아닌 무언가를 정리해야 할 때도 있을 것이다. 이 때는 finally 절을 사용하면 된다.

<br>

```java
try {
	작업을 수행한다.
} finally {
  정리 작업을 한다.
}
```

finally 절은 정상적으로든 예외가 일어나서든 try 블록이 끝날 때 실행된다.

<br>

finally 절에서는 예외를 던지지 말아야 한다. try 블록의 바디가 예외로 종료되어도 finally 절에서 일어난 예외로 가려지기 때문이다. 이와 같은 이유로 finally 절에는 return 문도 작성해서는 안된다. catch 절 뒤에 finally 절을 붙여서 try 문을 구성할 수도 있지만, finally 절에서 일어날 수 있는 예외에 주의해야 한다.

<br><br>

<h3>5.1.7 예외 다시 던지기와 예외 연쇄</h3>

---

예외가 일어날 때, 무슨 일을 해야 할지는 모르지만 실패를 로그로 기록하고 싶을 수 있다. 이때는 예외를 다시 던져 적합한 예외 핸들러가 다룰 수 있게 해주면 된다.

```java
try {
	작업을 수행한다.
} catch (Exception ex) {
  logger.log(level, message, ex);
	throw ex;
}
```

<br>

던져진 예외의 클래스를 변경하고 싶을 때도 있다. 예를 들어 서브시스템의 실패를 서브 시스템 사용자에게 의미 있는 예외 클래스로 보고해야 할 수도 있다. 서블릿 실행 중에 데이터베이스 오류가 일어났다면, 해당 서블릿을 실행하는 코드는 무엇이 잘못되었는지 자세히 알고 싶지 않을수도 있지만, 서블릿에 문제가 있다는 것은 확실히 알고 싶을 것이다. 따라서 이때는 원본 얘외를 잡아 상위 수준 예외로 연쇄해야 한다.

```java
try {
	데이터베이스에 접근한다.
} catch (SQLException ex) {
	throw new ServletException("database error", ex);
}

// ServletException을 잡을 때, 다음과 같이 예외의 원본을 추출할 수 있다.
Throwable cause = ex.getCause();
```

<br>

ServletException 클래스에는 예외의 원인을 매개변수로 받는 생성자가 있다. 물론 모든 예외 클래스에 이런 생성자가 있지는 않다. 이때는 다음과 같이 initCause 메서드를 호출해야 한다.

```java
try {
	데이터베이스에 접근한다.
} catch (SQLException ex) {
	Throwable ex2 = new CruftyOldException("database error");
	ex2.initCause(ex);
	throw ex2;
}
```

<br>

예외 클래스를 직접 작성한다면, 인수가 없는 생성자, 문자열을 받는 생성자 두개는 물론이고, 다음과 같은 생성자도 작성해야 한다.

```java
public class FileFormatException extends IOException {
	public FileFormatException() {}
	public FileFormatException(String message) {
		super(message);
	}
	public FileFormatException(Throwable cause) { initCause(cause); }
	public FileFormatException(String message, Throwable cause) {
		super(message);
		initCause(cause);
	}
}
```

<br>

**TIP**

<blockquote>

예외 연쇄 기법은 검사 예외를 허용하지 않는 메서드에서 검사 예외가 일어날 때도 유용하다. 해당 검사 예외를 잡아서 비검사 예외에 연쇄하면 된다.

</blockquote>

<br><br>

<h3>5.1.8 미처리 예외와 스택 추적</h3>

---

예외를 어디에서도 잡지 않으면 **스택 추적(stack trace)** 이 표시된다. 여기서 스택 추적이란, 예외가 던져진 지점에 대기 중이던 모든 메서드 호출의 목록이다. 스택 추적은 오류 메시지용 스트림인 System.err로 전달된다.

<br>

기술 지원 스태프의 조사용 등으로 예외를 다른 곳에 저장하고 싶다면, 기본 미처리 예외 (uncaught exception) 핸들러를 설정한다.

```java
Thread.setDefaultUncaughtExceptionHandler((thread, ex) -> {
	예외를 기록한다.
});
```

<br>

**Note**

<blockquote>

미처리 예외는 해당 예외가 일어난 스레드를 종료한다. 지금까지 살펴본 프로그램처럼 애플리케이션에 스레드가 한 개만 있다면, 프로그램은 미처리 예외 핸들러를 호출한  후 종료한다.

</blockquote>

<br>

가끔 예외를 반드시 잡아야 하지만 해당 예외로 무엇을 해야 할지 정말 모를 때가 있다. 예를 들어 Class.forName 메서드는 프로그래머가 처리해야 하는 검사 예외를 던진다. 무엇을 해야 할지 모르겠다면 예외를 무시하지 말고 스택 추적이라도 출력해야 한다.

```java
try {
	Class<?> cl = Class.forName(className);
	...
} catch (ClassNotFoundException ex) {
	ex.printStackTrace();
}
```

<br>

예외의 스택 추적 내용을 저장하고 싶을 때는 다음 문자열에 집어넣으면 된다.

```java
ByteArrayOutputStream out = new ByteArrayOutputStream();
ex.printStackTrace(new PrintWriter(out));
String description = out.toString();
```

<br>

**Note**

<blockquote>

스택 추적을 더 자세히 처리하고 싶다면 StackWalker 클래스를 이용하면 된다. 

</blockquote>

<br><br>

<h3>5.1.9 Objects.requireNonNull 메서드</h3>

---

Objects 클래스에는 편리한 매개변수 널 검사용 메서드가 있다. 다음은 이 메서드를 사용하는 예이다.

<br>

```java
public void process(String direction) {
	this.direction = Objects.requireNonNull(direction);
	...
}
```

위 코드에서 direction이 null이라면 NullPointerException이 일어난다. 따라서 개선되지 않은 것처럼 보이지만, 스택 추적을 거슬러 올라가는 방식을 생각하면 크게 개선된 방법이다. requireNonNull 호출을 문제의 원인으로 보면 무엇을 실수했는지 바로 알 수 있기 때문이다. 또한, 예외에 대응하는 메시지 문자열도 지정할 수 있다.

<br>

```java
this.direction = Objects.requireNonNull(direction, "direction must not be null");
// 위 메서드의 변형을 사용하면 예외를 던지게 하지 않고 대체 값을 전달할 수 있다.

this.direction = Objects.requireNonNullElse(direction, "North");

// 기본 값을 계산하는 비용이 많이 든다면, 또 다른 변형을 사용하는 것이 좋다.

this.direction = Objects.requireNonNullElseGet(direction, 
	() -> System.getProperty("com.horstMann.direction.default"));
// 이제 direction이 null일 때만 람다 표현식을 평가한다.
```

<br><br>

<h3>5.2.2 단정 활성화와 비활성화</h3>

---

단정은 기본적으로 비활성화되어 있다. 단정을 활성화하기 위해서는 -enableassertions나 -ea 옵션으로 프로그램을 실행해야 한다.

```java
java -ea MainClass
```

<br>

단정을 활성화하거나 비활성화하는 일은 클래스 로더가 처리하기에, 프로그램을 다시 컴파일 하지 않아도 된다. 또, 클래스 로더는 단정이 비활성화되어 있으면 단정 코드를 제거해서 프로그램 실행이 느려지지 않게 한다. 

<br>

```java
java -ea:MyClass -ea:com.mycompany.mylib... MainClass
// 특정 클래스나 전체 패키지에 단정을 활성화할 수도 있다.
```

위 명령어는 MyClass 클래스와 com.mycompany.mylib 패키지, 그 서브 패키지에 속한 모든 클래스에 단정을 활성화한다. 즉, -ea:… 옵션은 기본 패키지에 속한 모든 클래스에 단정을 활성화한다.

<br>

반대로, -disableassertions나 -da 옵션으로 특정 클래스와 패키지에 단정을 비활성화할 수도 있다.

```java
java -ea:... -da:MyClass MainClass
```

위 두개의 옵션을 사용하더라도 클래스 로더 없이 로드되는 ‘시스템 클래스’에는 적용되지 않는다. 시스템 클래스에 단정을 활성화하려면 -enablesystemassertions/esa 스위치를 사용해야 한다.

<br>

**프로그램에서 클래스 로더의 단정 상태를 제어할 수 있는 메서드**

<blockquote>

```java
void ClassLoader.setDefaultAssertionStatues(boolean enabled);
void ClassLoader.setClassAssertionStatus(String className, boolean enabled);
void ClassLoader.setPackageAssertionStatus(String packageName, boolean enabled);
```

setPackageAssertionStatus 메서드는 -enableassertions 명령줄 옵션과 마찬가지로 지정한 패키지와 그 서브 패키지의 단정 상태를 설정한다.
</blockquote>

<br><br>

<h2>5.3 로깅</h2>

---

보통 우리는 로그를 찍기 위해 System.out.println을 사용한다. 하지만, 이 방법을 사용하면 실행할 때, 적어놨던 코드를 검사 완료 후, 다시 제거해야 하는 과정을 필수로 실시해야 한다. 이를 대신하기 위해 로깅 API가 존재한다.

<br>

<h3>5.3.1 로거 사용</h3>

---

로깅 시스템(logging system)은 기본 로거(logger)를 이용한다(기본 로거는 Logger.getGlobal()을 호출해 얻는다).

```java
// 정보 메시지를 로그로 기록할 때는 info 메서드를 사용한다.
Logger.getGlobal().info("Opening file " + filename);
```

<br>

로그를 기록한 시각과 호출 클래스나 메서드의 이름이 자동으로 포함된다는 점에 주목하자. 하지만 다음과 같이 호출하면 info 메서드 호출이 효력을 잃는다.

```java
Logger.getGlobal().setLevel(Level.OFF);
```

<br>

**Note**

<blockquote>

이 예제에서는 로깅이 비활성화되어 있어도 “Opening file ” + filename 메시지가 생성된다. 메시지를 생성하는 비용이 염려된다면 람다 표현식을 사용하자.

```java
Logger.getGlobal().info(() -> “Opening file ” + filename);
```

</blockquote>

<br><br>

<h3>5.3.2 로거</h3>

---

전문적인 애플리케이션은 전역 로거 하나로 모든 레코드를 로그로 기록하려고 하지 않는다. 이 대신 로거를 직접 정의해서 사용한다. 지정한 이름으로 처음 로거를 요청하면 해당 로거가 생성된다.

<br>

```java
Logger logger = Logger.getLogger("com.mycompany.myapp");
```

이후에 같은 이름으로 로거를 요청하면 같은 로거 객체를 얻는다.

<br>

로거 이름은 계층 구조로 되어 있다. 로거의 부모와 자식은 특정 프로퍼티를 공유한다. 예로 “com.mycompany” 로거의 메시지를 끄면 자식 로거도 비활성화된다.

<br>

**Note**

<blockquote>

많은 프로젝트에서 사용자가 원하는 로깅 프레임워크를 끼워서 쓸 수 있는 SLF4J(www.slf4j.org) 같은 로깅 퍼사드(logging facade)를 이용한다. 하지만, 위에서 설명한 JDK일부인 java.util.logging 프레임워크도 다양한 상황에서 잘 작동하며 작동법을 잘 익혀 두면 좋다.

</blockquote>

<br>

**로깅 퍼사드**

<blockquote>

로깅 퍼사드란, 다른 로깅 프레임워크의 전면에 두고 사용하는 라이브러리를 의미한다. 퍼사드 패턴은 주로 복잡하게 얽힌 여러 API를 그대로 드러내기보다는 퍼사드 객체를 전면에 두고, 이를 이용해 간소화된 인터페이스로 접근하려는 목적에서 도입했다. 퍼사드 패턴은 세부 구현을 감추어서 의존도를 낮추고, 복잡한 API를 간소화해 사용하기 쉬운 형태로 제공할 수 있다는 장점이 있다.

</blockquote>

<br>

**Note**

<blockquote>

일부 JVM 모듈이 로깅용으로 사용하는 경량 System.Logger 인터페이스가 있다. 전체 JVM에서는 이런 로그를 java.util.logging으로 재지정하지만 다른 곳으로도 재지정할 수 있다. 이 기능은 애플리케이션 프로그래머용으로 만든 것이 아니므로 java.util.logging이나 로깅 퍼사드를 사용해야 한다.

</blockquote>

<br><br>

<h3>5.3.3 로깅 레벨</h3>

---

로깅 레벨은 일곱 가지로 SEVERE, WARING, INFO, CONFIG, FINE, FINER, FINEST가 있다. 기본 설정으로는 상위 레벨 세 개만 로그로 기록한다. 

<br>

```java
logger.setLevel(Level.FINE);
// 위처럼 로그가 기록되는 레벨을 다른 레벨로 설정할 수 있다.
```

<br>

이제 FINE 이상인 레벨을 모두 로그로 기록한다. Level.ALL로 모든 레벨의 로깅을 켜고, Level.OFF로 모든 로깅을 끌 수 있다. 각 레벨에 대응하는 로깅 메서드가 있다.

<br>

```java
logger.warning(message);
logger.fine(message);
```

<br>

만일 레벨이 바뀔 수 있다면 log 메서드를 사용하고 레벨을 인수로 전달하면 된다.

```java
Level level = ...;
logger.log(level, message);
```

<br>

**Tip**

<blockquote>

기본 로깅 설정은 레벨이 INFO 이상인 모든 레코드를 로그로 기록한다. 그러므로 사용자에게는 중요하지 않지만, 진단용으로는 유용한 디버깅 메시지를 로그로 남기려면 CONFIG, FINE, FINER, FINEST 레벨을 사용해야 한다.

</blockquote>

<br>

**Caution**

<blockquote>

로깅 레벨을 INFO보다 상세한 값으로 설정한다면 로그 핸들러 설정도 변경해야 한다. 기본 로그 핸들러는 레벨이 INFO 미만인 메시지를 무시한다.

</blockquote>

<br><br>

<h3>5.3.4 기타 로깅 메서드</h3>

---

```java
// 실행 흐름 추적용 편의 메서드
void entering(String className, String methodName)
void entering(String className, String methodName, Object param)
void entering(String className, String methodName, Object[] params)
void exiting(String className, String methodName)
void exiting(String className, String methodName, Object result)

```

<br>

ENTRY와 RETURN 문자열로 시작하는 FINER 레벨 로그 레코드를 만들어 내보자.

```java
public int read(String file, String pattern) {
	logger.entering("com.mycompany.mylib.Reader", "read", new Object[] { file, pattern });
	...
	logger.exiting("com.mycompany.mylib.Reader", "read", count);
	return count;
}
// 위와 같은 메서드는 가변 인수를 가진 메서드로 바꾸지 않는 것이 좋다.
```

<br>

보통 로깅을 사용하는 이유는 예기치 않은 예외를 로그로 기록하기 위함이다. 이를 위한 두가지 메서드는 로그 레코드에 예외 관련 설명을 포함한다.

```java
void log(Level l, String message, Throwable t)
void throwing(String className, String methodName, Throwable t)

// log 메서드는 보통 다음과 같이 사용한다.
try {
	...
} catch (IOException ex) {
	logger.log(Level.SEVERE, "Cannot read configuration", ex);
}

// throwing 메서드는 다음과 같이 사용한다.
if (...) {
	IOException ex = new IOException("Cannot read configuration");
	logger.throwing("com.mycompany.mylib.Reader", "read", ex);
	throw ex;
}
```

throwing 호출은 FINER 레벨로 로그 레코드를 기록하며, THROW로 시작하는 메시지를 포함한다.

<br>

**Note**

<blockquote>

기본 로그 레코드는 로깅을 호출한 메서드와 클래스의 이름을 호출 스택(call stack)에서 추론해서 보여준다. 하지만 가상 머신이 실행을 최적화하면 정확한 호출 정보를 얻지 못할 수도 있으므로, logp 메서드를 이용해야 한다.

```java
void logp(Level l, String className, String methodName, String message)
```

</blockquote>

<br>

**Note**

<blockquote>

다른 언어를 쓰는 사용자도 로깅 메시지를 이해할 수 있게 하기 위해서는 메시지를 지역화해야 한다.

```java
void logrb(Level level, ResourceBundle bundle, String msg, Object... params)
void logrb(Level level, ResourceBundle bundle, String msg, Throwable thrown)
```

</blockquote>

<br><br>

<h3>5.3.5 로깅 구성</h3>

---

구성 파일(configuration file)을 편집하면 로깅 시스템의 다양한 프로퍼티를 변경할 수 있다. 기본 구성 파일은 conf/logging.properties 이다. 다른 파일을 사용하기 위해서는 애플리케이션을 시작할 때 java.util.logging.config.file 프로퍼티를 원하는 파일 위치로 설정해야 한다.

```java
$ java -Djava.util.logging.config.file=configFile MainClass
```

<br>

**Caution**

<blockquote>

가상 머신이 시작 단계에서 main을 실행하기 전에 로그 관리자는(log manager)를 초기화하므로, main에서 System.setProperty(”java.util.logging.config.file”, configFile)을 호출하는 방법은 효력이 없다.

</blockquote>

<br>

기본 로깅 레벨을 변경하기 위해서는 conf/logging.properties에서

```java
.level= INFO
```

<br>

위 코드를 수정해야 한다.

```java
com.mycompany.myapp.level = FINE
```

<br>

또한, 위 코드를 추가하면 직접 만든 로거의 로깅 레벨을 지정할 수 있다. 즉, 로거 이름에 .level 접미어를 붙이면 된다.

<br>

콘솔에 메시지를 띄우는 것은 로거가 아니라 로그 핸들러가 하는 일이다. 로그 핸들러에도 레벨이 있기에, 

```java
java.util.logging.ConsoleHandler.level = FINE
```

콘솔에서 FINE 메시지를 확인하려면 위와 같이 설정해야 한다.

<br><br>

<h3>5.3.6 로그 핸들러</h3>

---

로거 : 레코드 → ConsoleHandler

ConsoleHandler : 레코드 → System.err 스트림에 출력.

<br>

구체적으로는 로거는 레코드를 부모 핸들러에게 전송하고, 궁극적인 조상 로거가 ConsoleHandler를 포함한다.

<br>

로드 핸들러에도 로깅 레벨이 존재하기에, 레코드를 로그로 기록하기 위해서는 해당 레코드의 로깅 레벨이 로거와 핸들러의 임계 값(threshold) 이상이어야 한다. 기본 로그 관리자 구성 파일(conf/logging.properties)에는 기본 콘솔 핸들러의 로깅 레벨이 INFO로 설정되어 있다.

<br>

FINE 레벨 레코드를 로그로 기록하려면 구성 파일에서 기본 로거 레벨과 핸들러 레벨을 함께 변경해야 한다. 구성 파일을 아예 사용하지 않고 직접 핸들러를 설치하는 방법도 있다.

<br>

```java
Logger logger = Logger.getLogger("com.mycompany.myapp");
logger.setLevel(Level.FINE);
logger.setUseParentHandlers(false);
Handler handler = new ConsoleHandler();
handler.setLevel(Level.FINE);
logger.addHandler(handler);
```

<br>

로거는 자신의 핸들러와 부모의 핸들러 모두에 레코드를 보낸다. 방금 만든 로거는 궁극적인 조상인 “”의 자손이다. “”로거는 INFO 레벨 이상인 모든 레코드를 콘솔에 보낸다. 하지만 이런 레코드를 두 번이나 보고 싶지는 않으므로 useParentHandlers 프로퍼티를 false로 설정했다.

<br>

로그 레코드를 다른 곳으로 보내려면 또 다른 핸들러를 추가해야 한다. 로깅 API는 이런 용도로 사용하는 FileHandler와 SocketHandler 핸들러를 제공한다. SocketHandler는 레코드를 지정한 호스트와 포트로 보낸다. 
여기서 더 관심 있는 핸들러는 레코드를 파일 하나로 모으는 FileHandler이다.

<br>

```java
FileHandler handler = new FileHandler();
logger.addHandler(handler);
// 위처럼 추가한 기본 파일 핸들러에 레코드를 보내기만 하면 된다.
```

<br>

레코드는 사용자 홈 디렉터리의 java*n*.log 파일로 보낸다(여기서 *n*은 파일을 고유하게 구분하는 숫자이다). 기본적으로 레코드는 XML 형식으로 만든다.

<br>

로그 관리자 구성에서 다양한 매개변수를 설정하거나 다음 생성자 중 하나를 사용해 파일 핸들러의 기본 동작을 바꿀 수 있다.

```java
FileHandler(String pattern)
FileHandler(String pattern, boolean append)
FileHandler(String pattern, int limit, int count)
FileHandler(String pattern, int limit, int count, boolean append)
```

<br>

여러 애플리케이션(또는 같은 애플리케이션의 여러 사본)에서 같은 로그 파일을 사용할 때는 append 플래그를 켜야 한다. 파일 이름 패턴에서 %u(고유 번호)를 사용해 각 애플리케이션이 고유한 로그 사본을 생성하는 방법도 있다.

<br>

파일 순환을 키는 방법도 좋다. 파일 순환을 키면 로그 파일이 myapp.log.0, myapp.log.1, … 등 순환 시퀀스로 관리된다. 파일의 크기 제한을 초과하면 가장 오래된 로그를 삭제하고, 나머지 파일은 이름을 변경한다. 이후 생성 번호가 0인 새 파일을 만든다.

<br><br>

<h3>5.3.7 필터와 포매터</h3>

---

로깅 레벨로 필터링하는 방법 외에도 Filter 인터페이스를 구현한 필터를 로거나 핸들러에 추가로 설치해 필터링하는 방법도 있다. Filter 인터페이스는 다음 메서드를 가진 함수형 인터페이스이다. 

```java
boolean isLoggable(LogRecord record)
```

<br>

로거나 핸들러에 필터를 설치하려면 setFilter 메서드를 호출해야 한다. 필터는 한 번에 하나만 설치할 수 있다.

<br>

ConsoleHandler와 FileHandler 클래스는 텍스트와 XML 형식으로 로그 레코드를 내보낸다. 하지만, 필요하면 원하는 포맷을 정의할 수도 있다. 원하는 포맷을 정의하려면 Formatter 클래스를 확장하고 다음 메서드를 오버라이드 해야 한다.

<br>

```java
String format(LogRecord record)
```

<br>

원하는 대로 레코드를 포매팅하고 결과로 나온 문자열을 반환하면 된다. format 메서드에서 다음 메서드를 사용하면 편리하게 포매팅할 수 있다.

```java
String formatMessage(LogRecord record)
```

<br>

formatMessage 메서드는 매개변수를 교체하고 지역화를 적용해 레코드의 메시지 부분을 포매팅한다.

<br>

XML 형식을 비롯한 많은 파일 형식에는 포맷 적용 레코드를 감싸는 헤드(head)(머리)와 테일(tail)(꼬리) 부분이 필요하다. 레코드에 헤드와 테일 부분을 제공하려면 다음 메서드를 오버라이드해야 한다.

```java
String getHead(Handler h)
String getTail(Handler h)
```

<br>

마지막으로 setFormatter 메서드를 호출해서 핸들러에 포매터를 설치한다.