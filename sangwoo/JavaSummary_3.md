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

<h2>3.1.2 인터페이스 구현</h2>

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
public class DigitSequence Implements IntSequence {
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
클래스가 인터페이스의 메서드 중 일부만 구현한다면, 해당 클래스는 반드시 abstract 제어자로 선언해야 한다.
</blockquote>

<br>

<h2>3.1.3 인터페이스 타입으로 변환</h2>

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

<h2>3.1.5 인터페이스 확장</h2>

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

<h2>3.1.6 여러 인터페이스 구현</h2>

---

클래스는 인터페이스를 몇 개든 구현 할 수 있다.

```java
public class FileSequence implements IntSequence, Closeable {
	...
}
// FileSequence는 IntSequence와 Closeable을 슈퍼타입으로 둔다.
```

<br>

<h2>3.1.7 상수</h2>

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