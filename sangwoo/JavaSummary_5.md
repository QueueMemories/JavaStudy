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