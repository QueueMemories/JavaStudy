**인터페이스를 사용하면 구현을 제공하지 않고서도 수행할 일을 명시할 수 있다.**

<h3>3장 핵심내용</h3>

1. 인터페이스는 구현 클래스에서 반드시 구현해야 하는 메서드를 명시한다.
2. 인터페이스는 해당 인터페이스를 구현하는 모든 클래스의 슈퍼타입이다. 따라서 구현 클래스의 인스턴스를 인터페이스 변수에 할당할 수 있다.
3. 인터페이스는 정적 메서드를 포함할 수 있다. 인터페이스의 모든 변수는 자동으로 public static final 이다.
4. 인터페이스는 구현 클래스에서 상속하거나 오버라이드할 수 있는 공개 메서드를 포함할 수 있다.
5. 인터페이스는 구현 클래스에서 호출하거나 오버라이드할 수 없는 비공개 메서드를 포함할 수 있다.
6. Comparable과 Comparator 인터페이스는 객체를 비교할 때 사용한다.
7. 함수형 인터페이스는 단일 추상 메서드를 가진 인터페이스다.
8. 람다 표현식은 나중에 실행할 수 있는 코드 블록이다.
9. 람다 표현식은 함수형 인터페이스로 변환된다.
10. 메서드 참조와 생성자 참조는 메서드와 생성자를 호출하지 않고 참조한다.
11. 람다 표현식과 지역 클래스는 자신을 감싸는 유효 범위에 있는 사실상 최종 변수에 접근할 수 있다.

<h2>3.1 인터페이스</h2>

---

<blockquote>서비스 공급자와 자신의 객체를 이 서비스에 사용하고 싶은 클래스 간의 계약을 기술하는 매커니즘.
</blockquote>
<br>

<h3>3.1.1 인터페이스 선언</h3>

---

```java
public static double average(IntSequence seq, int n)
// 위에서 IntSequence는 다양한 형태를 취할 수 있다.
```

위 IntSequence 는 다양한 형태를 취할 수 있기 때문에 이 형태들을 다룰 **단일 매커니즘**(single mechanism)을 구현한다.

<br>

**이를 다루기 위해 두 가지 메서드를 작성**

- 다음 요소가 있는지 검사하는 메서드
- 다음 요소를 얻는 메서드

<br>

```java
public interface IntSequence {
	boolean hasNext(); // 다음 요소가 있는지 검사하는 메서드
  int next();        // 다음 요소를 얻는 메서드
}
```

위처럼 구현은 하지 않고 선언만 한 메서드를 추상 메서드 (abstract method) 라고 한다.

<br>

****Note****
<blockquote>
인터페이스의 모든 메서드는 자동으로 public 이 된다. 따로 작성할 필요는 없지만 명확하게 드러내기 위해 작성할 수도 있다.
</blockquote>

<br>

<h3>3.1.2 인터페이스 구현</h3>

---

average 메서드에 사용할 수 있는 클래스들을 살펴본다. 이 클래스들은 IntSequence 인터페이스를 구현(implement)해야 한다.

```java
public class SquareSequence implements IntSequence {
	private int i;
    
	public boolean hasNext() {
		return true;
	}	

	public int next() {
		i++;
		return i*i;
	}
}

// 사각형 클래스
```

implements 키워드는 SquareSequence 클래스가 IntSequence 인터페이스를 따른다는 의미이다.

<br>

**Caution**
<blockquote>
인터페이스를 구현하는 클래스는 인터페이스의 메서드를 반드시 public 으로 선언해야 한다. 
</blockquote>

<br>

사각형 100개의 평균을 구하는 average

```java
public static double average(IntSequence seq, int n) {
	int count = 0;
	double sum = 0;
	while (seq.hasNext() && count < n) {
		count++;
		sum += seq.next();
	}
	return count == 0 ? 0 : sum / count;
}
```

<br>

다음 클래스는 유한 시퀀스(가장 낮은 자릿수부터 시작해 양의 정수를 구성하는 숫자)를 돌려준다.

```java
public class DigitSequence implements IntSequence {
	private int number;
	
	public DigitSequence(int n) {
		number = n;
	}

	public boolean hasNext() {
		return number != 0;
	}

	public int next() {
		int result = number % 10;
		number /= 10;
		return result;
	}

	public int rest() {
		return number;
	}
}
```

<br>

****Note****
<blockquote>
선언부만 있고, 구현부가 없는 메소드를 추상 메소드라고 하며 반드시 abstract 제어자를 붙여야 한다.
<br>
또한, 하나 이상의 추상 메소드를 포함하고 있는 추상 클래스도 반드시 abstract 제어자를 붙여야 한다.
<br>
따라서 클래스가 인터페이스의 메서드 중 일부만 구현한다면, 해당 클래스는 반드시 abstract 제어자로 선언해야 한다.
</blockquote>

<br>

<h3>3.1.3 인터페이스 타입으로 변환</h3>

---

```java
IntSequence digits = new DigitSequence(1729);
double avg = average(digits, 100);
// 처음 시퀀스 4개의 값만 보게됨.
```

위에서 digits 타입을 DigitSequence 로 하지 않고 IntSequence 로 하였다. IntSequence 타입 변수는 IntSequence 인터페이스를 구현한 어떤 클래스의 객체라도 참조할 수 있다. 

<br>

객체의 클래스가 인터페이스를 구현할 때는, 이 객체를 해당 인터페이스 타입 변수에 할당할 수 있다. 또, 이 객체를 해당 인터페이스를 기대하는 메서드에도 전달할 수 있다.

<br>

T 타입의 모든 값을 변환 없이 S 타입의 변수에 할당할 수 있다면, S 타입은 T 타입 (서브타입(subtype))의 슈퍼타입(supertype) 이다. 

ex) IntSequence 인터페이스는 DigitSequence 클래스의 슈퍼타입이다.

<br>

****Note****
<blockquote>
 인터페이스 타입으로 변수를 선언할 수 있지만, 타입이 인터페이스 자체인 객체는 만들 수 없다. 모든 객체는 클래스의 인스턴스이다. (선언은 가능하지만, 결국 클래스에서 구현된 인터페이스 메소드를 사용해야 하는거지)</blockquote>

 <br>

<h2>3.1.4 캐스트와 instanceof 연산자</h2>

---

보통 서브타입 → 슈퍼타입 의 흐름이 정상적이지만, 때때로 슈퍼타입 → 서브타입으로 변환해야 할 때도 있다. 이때는, 캐스트(cast : 강제변환) 을 사용하여 변환한다.

<br>

우리는 어떤 서브타입의 객체를 생성할 때, 슈퍼타입 으로 선언된 변수에 넣을 수 있다는 것을 안다. 이때, 만일 실제로 이 값을 사용해야 할 때는, 서브 타입의 객체로 선언된 변수로 접근해야 하므로 캐스트를 사용한다.

```java
IntSequence sequence = new DigitSequence(1729);
// 위처럼 할당했을 때,
// 실제로는 sequence가 DigitSequence의 객체라는 것을 안다면 cast를 할 수 있다.

DigitSequence digits = (DigitSequence) sequence;
// 이렇게 되면 digits.rest(); 를 사용할 수 있다.
```

위에서 말한 rest() 메서드는 IntSequence의 메서드가 아닌 DigitSequence의 메서드이기 때문에, 캐스트를 꼭 해주고 사용해야 한다.

<br>

객체 캐스트 조건

1. 실제 클래스
2. 슈퍼타입

<br>

이 외의 조건에서 캐스트 하게 되면, ClassCastException 오류가 발생한다.

```java
IntSequence sequence = new DigitSequence(1729);
String digitString = (String) sequence;
// 작동 불가! IntSequence가 String의 슈퍼타입이 아니기 때문!

RandomSequence randoms = (RandomSequence) sequence;
// 작동은 가능성은 있다.(public class RandomSequence implements IntSequence 로 선언했을 경우)
// 위가 아닐 시 캐스트 예외를 던짐.
```

<br>

**ClassCastException**

<blockquote>
위 오류를 피하기 위해서는 먼저 객체가 원하는 타입인지를 instanceof 연산자를 통해 검사해야 한다.


```java
object instanceof Type 
// object가 Type을 슈퍼타입으로 둔 클래스의 인스턴스 일때, true를 반환한다.

// 위 문장의 안정성을 살리기 위해,
if (sequence instanceof RandomSequence) {
	RandomSequence randoms = (RandomSequence) sequence;
} 
// sequence의 슈퍼타입이 RandomSequence면, 
// sequence의 타입을 RandomSequence 타입으로 캐스트 한다.
```

</blockquote>

<br>

****Note****
<blockquote>
instanceof 연산자는 null에 안전하다. 

obj가 null이면 → obj instanceof Type 표현식은 false 가 된다. null은 어떤 타입의 객체도 참조할 수 없음.
</blockquote>

<br>

<h3>3.1.5 인터페이스 확장</h3>

---

인터페이스는 또 다른 인터페이스를 확장(extend) 하여서 원래 있던 메서드 외에 추가 메서드를 요구하거나 제공할 수 있다.

```java
public interface Closeable {
	void close();
}
```

Closeable이라는 interface를 선언한다. 메소드는 한개만 존재.

<br>

```java
public interface Channel extends Closeable {
	boolean isOpen();
}
// Channel 인터페이스는 Channel 인터페이스를 확장한다.
```

위 Channel 인터페이스를 구현하는 클래스는 두 메서드를 모두 구현해야 하고, 두 인터페이스 타입 중 어느 것이든 해당 클래스의 객체를 변환할 수 있게 된다.

<br>

<h3>3.1.6 여러 인터페이스 구현</h3>

---

클래스는 인터페이스를 몇 개든 구현 할 수 있다.

```java
public class FileSequence implements IntSequence, Closeable {
	...
}
// FileSequence는 IntSequence와 Closeable을 슈퍼타입으로 둔다.
```

<br>

<h3>3.1.7 상수</h3>

---

인터페이스에 정의한 변수는 자동으로 public static final 이 된다. 

```java
public interface SwingContants {
	int NORTH = 1;
	int NORTH_EAST = 2;
	int EAST = 3;
}
// 이들은 모두 상수가 된다. 위에서는 나침반의 방향을 나타낸다.
```

<br>

****Note****
<blockquote>
인터페이스 안에는 인스턴스 변수를 둘 수 없다. 인터페이스는 객체의 상태가 아니라 동작 (behavior)을 명시한다.
</blockquote>

<br><br>

<h2>3.2 인터페이스의 정적 메서드, 기본 메서드, 비공개 메서드</h2>

---


자바 8 이전에는 인터페이스의 모든 메서드가 추상 메서드여야만 했다. 하지만 자바 9에서는 실제 구현이 있는 메서드 세 종류 (정적 메서드, 기본 메서드, 비공개 메서드)를 인터페이스에 추가할 수 있다.

<br>

<h3>3.2.1 정적 메서드</h3>

---

ex) IntSequence 인터페이스에는 주어진 정수의 숫자 시퀀스를 만들어 내는 정적 메서드인 digitsOf를 둘 수 있다.

```java
IntSequence digits = IntSequence.digitsOf(1729);
// 위 메서드는 IntSequence 인터페이스를 구현한 클래스의 인스턴스를 돌려주지만,
// 호출자는 이 인스턴스가 어느 클래스의 인스턴스인지 신경 쓸 필요는 없다.

public interface IntSequence {
	...
	static IntSequence digitsOf(int n) {
		return new DigitSequence(n);
	}
}
```

<br>

**역주**
<blockquote>
자바 8부터는 정적 메서드와 기본 메서드를 인터페이스에 넣을 수 있고, 자바 9부터는 비공개 메서드를 인터페이스에 넣을 수 있다.
</blockquote>

<br>

<h3>3.2.2 기본 메서드</h3>

---

인터페이스에 있는 어느 메서드에서든 default 제어자를 붙이면, 기본 구현을 작성할 수 있다.

```java
public interface IntSequence {
	default boolean hasNext() { return true; }
	// 연속된 숫자는 무한임을 표현함.
	int next();
}
```

이제 이 인터페이스를 구현하는 클래스는 hasNext 메서드를 오버라이드하거나 기본 구현을 상속하는 방법 중 하나를 선택할 수 있다.

<br>

**Note**
<blockquote>
이 기본 메서드 덕분에 인터페이스와 해당 인터페이스의 메서드를 대부분 또는 모두 구현한 동반 클래스를 제공하던 고전적인 패턴에 종지부를 찍을 수 있다. 이제는 인터페이스에 이런 메서드를 바로 구현한다.
</blockquote>

<br>

<h3>기본 메서드의 주요 용도는 인터페이스를 진화(interface evolution) 시키는 것이다.</h3>
<blockquote>

```java
public class Bag implements Collection
// 자바 8 이전에 위 클래스를 작성했다고 가정.
```

이후에 Collection 인터페이스에 stream 메서드를 추가했고 stream은 기본 메서드가 아닐 때, Bag 클래스는 새로 추가된 메서드를 구현하지 않기에 컴파일되지 않는다.

즉, 인터페이스에 기본 메서드가 아닌 메서드를 추가하면 **소스 수준에서 호환(source-compatible)** 되지 않는다.

하지만, 클래스를 다시 컴파일하지 않고 Bag 클래스가 포함된 기존 JAR 파일을 그대로 사용한다면, 구현하지  않은 메서드가 있음에도 여전히 클래스를 로드한다. 프로그램에서 여전히 Bag 인스턴스를 생성할 수 있고, 문제도 전혀 없다. 

즉, 인터페이스에 메서드를 추가하는 것은 
**바이너리 수준에서 호환(binary-compatible)**
된다.

그렇다 하더라도, 프로그램에서 Bag 인스턴스로 stream 메서드를 호출하면 AbstractMethodError를 던진다.

위 문제를 해결하기 위해 메서드를 default로 선언해야 한다. 즉, Bag 클래스는 이제 제대로 컴파일 된다. 또, Bag 클래스를 다시 컴파일하지 않고 로드한 후 Bag 인스턴스로 stream 메서드를 호출하면 정상적으로Collection.stream 메서드가 호출된다.
</blockquote>

<br>

<h3>3.2.3 기본 메서드의 충돌 해결</h3>

---

```java
// getId 메서드를 가진 Person 인터페이스가 있다고 가정,
public interface Person {
	String getName();
	default int getId() { return 0; }
}

// Identified 인터페이스도 getId를 기본 메서드로 가진다고 가정
public interface Identified {
	default int getId() { return Math.abs(hashCode()); }
}
```

<br>

위 두 개의 인터페이스를 구현하는 클래스를 만든다.

```java
public class Employee implements Person, Identified {
	...
}
```

위 코드를 작성하면, 자바 컴파일러가 두 기본 메서드 중 하나를 우선해서 선택하지 못한다. 따라서 오류를 던진다.

<br>

해결방법

1. Employee 클래스에 getId 메서드를 추가한 후 고유의 ID 체계를 구현
2. 충돌한 메서드 중 하나에 위임.

```java
public class Employee implements Person, Identified {
	public int getId() { return Identified.super.getId(); }
}
```

<br>

**Note**
<blockquote>
super 키워드로 슈퍼타입의 메서드를 호출할 수 있다. 따라서 위 예제에서 둘 중 어느 슈퍼타입의 기본 메서드를 원하는지 명시해야 오류를 피할 수 있다.
</blockquote>

<br>

그렇다면, 한쪽에서 기본 메서드로 명시하지 않은 경우에는 어떻게 될까

```java
interface Identified {
	int getId();
}
// 위처럼 Identified 인터페이스에서 getId를 기본 메서드로 구현하지 않는다고 가정,
```

이 때의 컴파일러는 Iendtified.getId가 수행할 동작을 Person.getId 메서드가 실제로 하는지 알 수 없다. 이에 자바 설계자들은 안전성과 일관성을 따르기로 했다. 두 인터페이스가 어떻게 충돌하는지는 중요하지 않고 적어도 한 인터페이스에서 구현을 제공하면 컴파일러는 오류를 보고하며, 모호성을 해결하는 일은 프로그래머의 책임이다.

<br>

<h3>3.2.4 비공개 메서드</h3>

---

비공개 메서드는 static 이나 인스턴스 메서드는 될 수 있지만, default 메서드는 (오버라이드가 가능하므로) 될 수 없다. 비공개 메서드는 인터페이스 자체에 있는 메서드에서만 쓸 수 있으므로, 인터페이스 안에 있는 다른 메서드의 헬퍼(helper) 메서드로만 사용할 수 있다.

```java
// IntSequence 인터페이스가 다음 메서드를 제공한다고 할 때,
static of(int a);

// 위 메서드는 다음 헬퍼 메서드를 호출할 수 있다.
private static IntSequence makeFiniteSequence(int ... values) { ... }
```

<br>

<h2>3.3 인터페이스의 예</h2>

---

**인터페이스는 그저 클래스가 구현하기로 약속한 메서드 집합이다.**

<br>

<h3>3.3.1 Comparable 인터페이스</h3>

---

예시로 정렬 알고리즘이 있을 때, 비교하는 방식은 클래스마다 다를 것이다. 하지만 제공하는 기능은 같아야 하기 때문에, 모든 클래스가 호출될 메서드를 무엇으로 할지 합의하면 정렬 알고리즘은 정렬을 수행할 수 있다. 이 때, Comparable 인터페이스를 사용한다.

```java
public interface Comparable<T> {
	int compareTo(T other);
}
// 비교 대상 타입이 다를 수 있기에, 타입 매개변수를 받는다.

x.compareTo(y);
// 위를 호출하면 compareTo는 x와 y중 어느 것이 앞에 오는지 나타낸다.
// 양수면 : x > y, 음수 : y > x, 0 : x == y
```

<br>

어떤 정수든 반환 값이 될 수 있기에, 정수 간 차이를 반환할 수 있다.

```java
public class Employee implements Comparable<Employee> {
	...
	public int compareTo(Employee other) {
		return getId() - other.getId(); 
	}
}
// ID가 항상 0 이상이여야 정상 작동. 
```

<br>

**Caution**

<blockquote>

**Integer.compare** - 두 정수의 차이가 오버플로될 수 있는 문제를 해결하는 방법. 모든 정수에 올바르게 작동한다.

**Double.compare** - 부동소수점 값을 비교할 때는, 바로 반환하지 않고 정적 메서드인 이 메서드를 사용.
</blockquote>

<br>

직원들을 급여 순으로 정렬하는 법!

```java
public class Employee implements Comparable<Employee> {
	...
	public int compareTo(Employee other) { 
		return Double.compare(salary, other.salary)
	}
}
```

<br>

**Note**

<blockquote>

compare 메서드가 other.salary에 접근하는 것은 규칙에 들어맞는다. 자바의 메서드는 자신이 속한 클래스의 모든 객체에 있는 비공개 기능에 접근할 수 있다.

String 클래스에 존재하는 Arrays.sort 메서드는 Comparable 객체로 구성된 배열을 정렬한다.

</blockquote>

<br>

**Note**

<blockquote>

Arrays.sort 메서드는 컴파일 시간에 인수가 Comparable 객체의 배열인지 검사하지 않는다. 대신 Comparable 인터페이스를 구현하지 않은 클래스 요소를 만나면 예외를 던진다.

</blockquote>

<br>

<h3>3.3.2 Comparator 인터페이스</h3>

---

String 클래스는 comparTo 메서드를 두 가지 방법으로 구현하지 못한다. 이런 상황을 다룰 수 있는 Arrays.sort 메서드의 두 번째 버전이 있다. 배열과 비교자를 매개변수로 받는다.

```java
public interface Comparator<T> {
	int compare(T first, T second);
}
```

<br>

문자열을 길이로 비교하기 위해서는 Comparator<String>을 구현하는 클래스를 정의해주어야 한다.

```java
class LengthComparator implements Comparator<String> {
	public int compare(String first, String second) { 
		return first.length() - second.length();
	}
}
```

<br>

실제 비교를 위해서는 이 클래스의 인스턴스(객체)를 만들어야 한다.

```java
Comparator<String> comp = new LengthComparator();

if (comp.compare(words[i], words[j]) > 0) …
// 위에서 compare 메서드는 문자열 자체가 아니라 비교자 객체로 호출한다.
```

<br>

**Note**

<blockquote>

LengthComparator 객체에는 상태가 없지만, compare 메서드는 정적 메서드가 아니므로 compare 메서드를 사용하고 싶다면 인스턴스가 필요하다.

</blockquote>

<br>

만약, 배열을 정렬하고 싶다면 LengthComparator 객체를 Arrays.sort 메서드에 전달해야 한다.

```java
Arrays.sort(friends, new LengthComparator());
```

<br>

<h3>3.3.3 Runnable 인터페이스</h3>

---

태스크를 정의하기 위해 Runnable 인터페이스를 구현해야 한다. Runnable 인터페이스는 메서드가 한 개만 존재한다.

```java
class HelloTask implements Runnable {
	public void run() {
		for (int i = 0; i < 1000; i++) {
			System.out.println("Hello, World!");
		}
	}
}
```

위 Hello, World를 1000번 출력하는 태스크를 새 스레드에서 실행하기 위해서는 Runnable로 스레드를 생성하고 시작해야 한다.

```java
Runnable task = new HelloTask();
Thread thread = new Thread(task);
thread.start();
```

이제 run 메서드는 별도의 스레드에서 실행되기에 현재 스레드는 다른 작업을 계속할 수 있다.

<br>

**Note**

<blockquote>

T 타입 결과를 반환하는 태스크에 사용하는 Callable<T>라는 인터페이스도 존재한다.

</blockquote>

<br>

<h3>3.3.4 사용자 인터페이스 콜백</h3>

---

어떤 이벤트가 발생했을 때, 수행할 액션(action)(동작)을 지정해야 한다. 이런 액션을 콜백(callback)이라고 하는데, 사용자가 어떤 액션을 취하면 미리 지정해둔 코드를 역으로 호출하는 방식이다.

<br>

자바 기반으로 된 GUI 라이브러리에서는 콜백에 인터페이스를 사용한다. 
<br>
ex) JavaFX에서는 이벤트를 보고할 때, 다음 인터페이스를 사용함.

```java
public interface EventHandler<T> {
	void handle(T event);
}
// 여기서 T는 보고할 이벤트 타입.
```

<br>

수행할 액션을 지정하기 위해서는 EventHandler<T> 인터페이스를 구현해야 한다.

```java
class CancelAction implements EventHandler<ActionEvent> {
	public void handle(ActionEvent event) {
		System.out.println("Oh noes!");
	}
}
```

<br>

구현 후에, 이 클래스의 객체를 생성하여 버튼에 추가한다.

```java
Button cancelButton = new Button("Cencel");
cancelButton.setOnAction(new CancelAction());
```

<br>

**Note**

<blockquote>

모든 사용자 인터페이스 툴킷에서 버튼을 클릭했을 때, 실행할 코드를 해당 버튼에 전달해야 한다는게 가장 중요하다.

</blockquote>

<br><br>

<h2>3.4 람다 표현식</h2>

---

람다 표현식 (lambda expression)은 나중에 한 번 이상 실행할 수 있게 전달하는 코드 블록이다. 

<br>

코드 블록을 지정할 때 유용한 상황들.

1. Arrays.sort에 비교 메서드 전달
2. 별도의 스레드에서 태스크 실행
3. 버튼을 클릭했을 때, 일어날 액션 지정

<br>

but 자바는 모든 것이 객체인 객체 지향 언어. 따라서 함수 타입이 존재하지 않지만, 이를 객체(특정 인터페이스를 구현하는 클래스의 인스턴스)를 통해 표현한다.

<br>

<h3>3.4.1 람다 표현식 문법 </h3>

---

```java
// 자바는 타입 결합이 강한 언어.
(String first, String second) -> first.length() - second.length()
```

위는 람다 표현식(코드 블록)으로, 해당 코드에 전달해야 하는 변수의 명세(specification)까지 갖추고 있다.

<br>

**람다 표현식 == 매개변수가 존재하는 표현식**

<br>

만일, 람다 표현식의 바디에서 표현식 하나로는 표현할 수 없는 계산을 수행한다면, 메서드를 쓸 때처럼 { } 로 감싸고 명시적인 return 문을 사용한다.

```java
(String first, String second) -> {
	int difference = first.length() - second.length();
	if (difference < 0) return -1;
	else if (difference > 0) return 1;
	else return 0
}

// 매개 변수가 없는 메서드는 빈 괄호를 붙여야 함.
() -> {}; 

// 만일 매개변수 타입을 추론할 수 있다면, 매개변수 타입을 생략할 수 있음.
Comparator<String> comp 
	= (first, second) -> first.length() - second.length();
```

매개변수 타입을 생략한 코드는 람다 표현식을 문자열 비교자(Comparator<String>)에 할당하기에, 컴파일러는 매개변수의 타입을 자동으로 인식한다.

<br>

매개변수가 하나 있고, 매개변수 타입을 추론할 수 있다면, 괄호까지 생략할 수 있다.

<br>

위 코드를 보면 알 수 있듯이, 람다 표현식의 결과 타입은 명시하지 않지만, 컴파일러는 이를 추론한 후 검사한다.

<br>

<h3>3.4.2 함수형 인터페이스</h3>

---

Runnable 이나 Comparartor 처럼 액션을 표현하는 인터페이스가 있다.

람다 표현식은 이런 인터페이스와 호환된다.

<br>

<blockquote>
람다 표현식은 단일 추상 메서드(single abstract method)를 가진 인터페이스(추상 메서드가 한개만 존재하는 인터페이스) 자리에 사용할 수 있고, 이러한 인터페이스를 함수형 인터페이스(function interface)라고 한다.
</blockquote>

<br>

함수 리터럴을 지원하는 거의 모든 프로그래밍 언어에서 (String, String) → int 처럼 함수 타입을 선언하고, 이 함수 타입으로 변수를 선언한 후, 함수를 변수에 저장해 호출할 수 있다. 

<br>

하지만, 자바에서는 이 중 하나만 람다 표현식으로 할 수 있다.

바로 람다 표현식을 함수형 인터페이스 타입 변수에 저장하여 해당 인터페이스의 인스턴스로 변환하는 것이다.

<br><br>

<h2>3.5 메서드 참조와 생성자 참조</h2>

---

다른 코드에 전달하려는 액션을 수행하는 메서드가 이미 있을 수도 있다. 이 때 사용하는 메서드 참조 (method reference)용 특수 문법을 알아본다.

<br>

**즉, 메서드 참조는 람다 식에서 파라미터의 중복을 피하기 위해 사용한다. 파라미터가 중복되지 않았다면 사용 불가.**

<br>

<h3>3.5.1 메서드 참조</h3>

---

대, 소문자 구분 없이 문자열을 정렬하고자 하면, 다음 코드를 사용할 수 있다.

```java
Arrays.sort(strings, (x, y) -> x.comareToIgnoreCase(y));
// or
Arrays.sort(strings, String::compareToIgnoreCase);
```

 두번째 코드에서 쓰인 String::compareToIgnoreCase는 (x, y) -> x.comareToIgnoreCase(y)에 대응하는 메서드 참조이다.

 <br>

```java
// 리스트에서 null 값을 모두 제거하는 예
list.removeIf(Objects::isNull);

// 리스트트의 모든 요소를 출력하는 예
list.forEach(x -> System.out.println(x));

// 하지만 println 메서드를 forEach 메서드에 전달하는게 더 좋다.
list.forEach(System.out::println);

```

<br>

**:: 연산자**

1. Class :: instanceMethod
    
    첫 번째 매개변수가 메서드의 수신자가 된다. 나머지 매개변수는 메서드에 전달한다.
    
2. Class :: staticMethod
    
    모든 매개변수가 정적 메서드로 전달된다. ( Objects::isNull == Objects.isNull(x) ) 
    
3. object :: instanceMethod
    
    주어진 객체로 메서드를 호출한다. ( System.out::println == x → System.out.println(x) )
    

    <br>

**Note**
<blockquote>
그렇다면, 같은 이름으로 오버로도드 된 함수가 여러개 있을 때는 어떻게 될까?

컴파일러가 스스로 어느 것을 의도했는지 문맥으로 알아낸다. ( int 인지 String 인지에 대해 )

또한, 메서드 참조에서 this 매개변수를 캡처할 수 있다. ( this::equals == x → this.equals(x) )

</blockquote>

<br>

<h3>3.5.2 생성자 참조</h3>

---

메서드 이름이 new 인것만 제외한다면, 메서드 참조와 같다. Employee::new 는 Employee의 생성자 참조이다.

<br>

메서드 참조와 똑같음.

```java
List<Member> memberList 
	= memberNameList.stream() 
	.map(name -> new Member(name)) 
	.collect(Collectors.toList());

List<Member> memberList 
	= memberNameList.stream() 
	.map(Member :: new)
	.collect(Collectors.toList());
// 위의 람다식을 줄여 아래의 생성자 참조를 통해 코드를 줄일 수 있다.
```

<br>

배열 타입도 생성자 참조가 가능하다. int[]::new == n → new int[n]

```java
Employee[] buttons = stream.toArray(Employee[]::new);
// 위에서 toArray 메서드는 이 생성자 참조가 가리키는 생성자를 호출한 후,
// 올바른 타입의 배열을 생성하고 해당 배열에 내용을 채워 반환한다.
```

<br><br>

<h2>3.6 람다 표현식 처리</h2>

---

이번에는 람다 표현식을 소비할 수 있는 메서드를 작성할 수 있는 방법을 알아본다.

<br>

<h3>3.6.1 지연 실행 구현</h3>

---

람다를 사용하는 핵심 목적은 **지연 실행**(deferred exccution)이다. 내가 쓰고 싶을 때 쓸 수 있다느 장점이 있다. 이것을 안 지키려면 그냥 람다로 감쌀 필요 없이 바로 실행하면 되기 때문이다.

<br>

**코드를 나중에 실행하는 이유**

- 별도의 스레드에서 코드 실행
- 코드를 여러 번 실행
- 알고리즘에서 적절한 시점에 코드 실행 ( 정렬에서 비교 연산 같은 )
- 어떤 행위 ( 버튼 클릭, 데이터 수신 등 ) 이 일어날 때 코드 실행
- 필요할 때만 코드 실행

<br>

어떤 액션을 n번 반복하고자 하는 예제.

```java
repeat(10, () -> System.out.println("안녕하세요."));

// repeat 메서드에서 람다를 받기 위해 함수형 인터페이스를 선택 or 드물게는 구현을 해야 한다.
// 이 예제에서는 간단하게 Runnable 인터페이스를 사용한다.
public static void repeat(int n, Runnable action) {
	for (int i = 0; i < n; i++) action.run();
}
// 람다 표현식의 바디는 action.run()이 호출될 때, 실행된다는 점을 염두에 둔다.
```

<br>

<h3>3.6.2 함수형 인터페이스 선택</h3>

---

함수형 프로그래밍 언어는 대부분 함수 타입이 **구조적(structural)**이다. <br>
<blockquote>
여기서 구조적이란, 언어의 타입 시스템에서 타입을 구조로 구분한다는 뜻이고, 이는 타입을 이름으로 구분하는 명목적 타입 지정과 대조를 이룬다.
</blockquote>

따라서, 문자열 두 개를 정수를 대응시키는 함수를 만들 때는 Function2<String, String, Integer> or (String, String) → int 형태의 타입을 사용한다. 

<br>

하지만, 자바에서는 Comparator<String> 같은 함수형 인터페이스 의도를 선언한다. 프로그래밍 언어 이론에서 이 방식을 **명목적 타입 지정(nominal typing)** 이라고 한다.

<br>

**자주 사용하는 함수형 인터페이스**

<img src="https://velog.velcdn.com/images/koo9b9h/post/2914fde5-1475-4d91-941e-cf78441424ca/image.png">

<br>

예를 들어, 특정 기준에 맞는 파일을 처리하는 메서드를 작성하고자 한다. 이때, 의도를 나타내는 java.io.FileFilter 클래스와 Predicate<File> 중 어느 것을 사용하는 것이 맞을까 ?

FileFilter 인스턴스를 만들어 내는 메서드가 많은 상황이 아닐 때는 표준 Predicate<File>을 사용하는 것이 더 좋은 방법이다.

<br>

**Note**

<blockquote>
대부분의 표준 함수형 인터페이스에는 함수를 생성하거나 결합하는 비추상 메서드가 존재한다. 

```java
Predicate.isEqual(a).or(Predicate.isEqual(b))
// ==
x -> a.equals(x) || b.equals(x)
```

</blockquote>

<br>

**기본 타입용 함수형 인터페이스**

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR02NUIVo2pIL3VZbotGpOOB3-g70nszrysow&usqp=CAU">

 \* p, q는 int, long, double 이고 P, Q는 Int, Long, Double 이다.

<br>

<h3>3.6.3 독자적인 함수형 인터페이스 구현</h3>

---

만일 표준 함수형 인터페이스가 적합하지 않은 상황이라면, 인터페이스를 직접 구현해야 한다.

<br>

**Note**
<blockquote>
함수형 인터페이스에는 @FunctionalInterface 애너테이션을 붙이면 좋다.

장점

1. 애너테이션이 붙은 엔터티(entity)가 추상 메서드를 한 개만 가진 인터페이스인지 컴파일러가 검사해준다. (함수형 인터페이스에는 추상 메서드가 한개만 존재)
2. 해당 인터페이스가 함수형 인터페이스라는 문장이 자바독 페이지에 추가된다.

</blockquote>