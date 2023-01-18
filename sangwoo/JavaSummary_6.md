<h1>6장 제네릭 프로그래밍</h1>

---

다양한 타입에서 작동하는 메서드와 클래스를 구현해야 할 때가 있다. 이때 사용하는 것이 제네릭이다.
ArrayList\<T>에서 ArrayList 클래스를 **제네릭(generic)** 이라고 하고, T를 타입 **매개변수**라고 한다.

<br>

<h2>6장 핵심내용</h2>

---

- 제네릭 클래스는 타입 매개변수를 한 개 이상 받는 클래스다.
- 제네릭 메서드는 타입 매개변수를 받는 메서드다.
- 타입 매개변수가 타입 한 개 이상의 서브타입이 되도록  요구할 수 있다.
- 제네릭 타입은 불변 타입이다. 즉, S가 T의 서브타입일 때, G\<S>와 G\<T> 사이에는 아무런 관련이 없다.
- 와일드카드 G<? extends T> 또는 G<? super T>를 사용해 메서드가 서브클래스나 슈퍼클래스 인수로 넘긴 제네릭 타입의 인스턴스를 수용할 수 있음을 명시할 수 있다.
- 제네릭 클래스와 제네릭 메서드를 컴파일할 때 타입 매개변수가 소거된다(지워진다).
- 타입 소거로 제네릭 타입에 많은 제약이 생긴다. 특히 제네릭 클래스나 배열의 인스턴스 생성, 제네릭 타입으로 캐스트, 제네릭 타입 객체를 예외로 던지는 일을 할 수 없다.
- Class\<T> 클래스는 제네릭이다. 덕분에 cast 같은 메서드가 T 타입 값을 만들어 내도록 선언되어 유용하다.
- 제네릭 클래스와 제네릭 메서드의 타입 매개변수는 가상 머신에서 소거되지만, 실행 시간에 이들이 어떻게 선언되어 있는지 알아낼 수 있다.

<br><br>

<h2>6.1 제네릭 클래스</h2>

---

**제네릭 클래스 (generic class)** 는 **타입 매개변수(type parameter)** 가 한 개 이상 있는 클래스다.

<br>

```java
// 키 & 값 쌍을 저장하는 클래스
public class Entry<K, V> {
	private K key;
	private V value;

	public Entry(K key, V value) {
		this.key = key;
		this.value = value;
	}

	public K getKey() { return key };
	public V getValue() { return value };
}
```

위에서 클래스 이름(Entry) 뒤에 오는 <> 안에 매개변수 K와 V를 명시했다. 이는 인스턴스 변수, 메서드 매개변수, 반환 값의 타입으로 사용된다.

<br>

위에서 프로그래머는 타입 변수를 원하는 타입으로 교체해 제네릭 클래스를 인스턴스화한다. 즉, 구체적인 인스턴스로 만든다.

<br>

**Caution**

<blockquote>

기본 타입으로는 타입 매개변수를 인스턴스화 할 수 없다.

</blockquote>

<br>

제네릭 클래스의 객체를 생성(construct)할 때, 생성자의 타입 배개변수를 생략할 수 있다.

```java
Entry<String, Integer> entry = new Entry<>("Fred", 42);
// new Entry<String, Integer>("Fred", 42); 와 같다.
```

그렇더라도, 다이아몬드(diamond)라고 불리우는 <>은 불여주어야 한다. 이 문법을 사용해야 생성자의 타입 매개변수가 추론된다.

<br><br>

<h2>6.2 제네릭 메서드</h2>

---

제네릭 클래스가 타입 매개변수를 받는 클래스인 것 처럼, **제네릭 메서드(generic method)** 는 타입 매개변수를 받는 메서드이다. 이는 일반 클래스나 제네릭 클래스에 속할 수 있다.

<br>

```java
// 일반 클래스에 속한 제네릭 메서드의 예
public class Arrays {
	public static <T> void swap(T[] array, int i, int j) {
		T temp = array[i];
		array[i] = array[j];
		array[j] = temp;
	}
}

// 배열에 들어 있는 요소의 타입이 기본 타입만 아니라면 swap 메서드로,
// 임의의 배열에 들어 있는 요소를 교환할 수 있다.
String[] friends = ...;
Arrays.swap(friends, 0, 1);
```

<br>

제네릭 메서드를 선언할 때는 타입 매개변수를 제어자(public 이나 static 같은)와 반환 타입 사이에 두어야 한다.

```java
public static <T> void swap(T[] array, int i, int j)
```

<br>

제네릭 메서드를 호출할 때는,  타입 매개변수를 명시하지 않아도 된다. 컴파일러가 메서드 매개변수와 반환 타입에서 타입 매개변수를 추론하기 때문이다.

<br>

즉, Arrays.swap(friends, 0, 1); 이 호출에서 friends의 타입이 String[]이므로 컴파일러는 T가 String이라고 추론한다. 아래와 같이 명시적으로 타입을 지정하고 호출해도 상관없다.

```java
Arrays.<String>swap(friends, 0, 1);
```

<br>

타입을 명시적으로 지정하면 문제가 생겼을 때, 더 자세한 오류 메시지를 얻을 수 있다.

<br><br>

<h2>6.3 타입 경계</h2>

---

제네릭 클래스나 제네릭 메서드가 받는 타입 매개변수의 타입을 제한해야 할 때, 타입 경계(type bound)를 지정하면 타입이 특정 클래스를 확장하거나 특정 인터페이스를 구현할 수 있다.

<br>

예를 들어 AutoCloseable 인터페이스를 구현하는 클래스의 객체로 구성된 ArrayList가 있고, 이 리스트에 들어 있는 객체를 모두 닫는다고 가정한다.

```java
public static <T extends AutoCloseable> void closeAll(ArrayList<T> elems)
				throws Exception {
	for (T elem : elems) elem.close();
} 
```

<br>

위에서 타입 경계로 쓰인 extends AutoCloseable은 요소 타입이 AutoCloseable의 서브타입임을 보장한다. 따라서, 이 메서드에 ArrayList<PrintStream>은 전달할 수 있지만, ArrayList<String>은 전달할 수 없다. 타입 경계에서 쓰이는 extends 키워드는 ‘서브타입’을 의미한다.

<br>

다음과 같이 타입 매개변수에 다중 경계를 지정할 수도 있다.

```java
T extends Runnable & AutoCloseable
```

<br>

이 문법은 여러 예외를 잡아내는 문법과도 유사하다. 차이점이 있다면 여러 예외는 | 연산자로 결합하지만, 타입 경계로 사용하는 타입은 & 연산자로 결합한다는 것이다.

<br>

인터페이스 경계는 원하는 개수만큼 지정할 수 있지만, 클래스 경계는 한 개만 지정할 수 있다. 클래스를 경계로 지정할 때는 경계 목록에서 첫 번째에 두어야 한다.

<br><br>


<h2>6.4 타입 가변성과 와일드카드</h2>

---

만일 어떤 클래스의 서브 클래스 객체로 구성된 배열을 처리하는 메서드를 구현한다면 어떻게 해야 할까? 이렇다면 간단하게 슈퍼클래스 타입으로 선언하면 된다.

<br>

즉, Manager가 Employee의 서브 클래스면 Manager[] 도 Employee[]의 서브타입이므로 Manager[] 배열을 이 메서드에 전달할 수 있다. 이를 **공변성(covariance)** 이라고 한다. 즉, 배열은 요소 타입과 같은 방식으로 변한다.

<br>

그렇다면, 배열 리스트를 처리할 때는 어떨까. ArrayList<Manager>는 ArrayList<Employee>의 서브타입이 아니다. 이렇게 할당하면 관리자가 아닌 직원을 저장할 때, 배열 리스트에 손상을 줄 수 있다.

<br>

```java
ArrayList<Manager> bosses = new ArrayList<>();
ArrayList<Employee> empls = bosses; // 규칙에 어긋난다. 예시를 보여주기 위함.
empls.add(new Emplyee(...)); // bosses에 관리자가 아닌 직원을 집어넣는다.
```

위 코드에서 ArratList<Manager> 가 ArrayList<Employee>로 변환하는 것을 허용하지 않으므로 이는 일어날 수 없다.

<br>

**Note**

<blockquote>

배열에서도 마찬가지로 Manager[] 배열에 Employee를 저장하면 ArrayStoreException 오류가 일어난다. 반면 자바의 모든 제네릭 타입은 불변(invariant)한다.

</blockquote>

<br>

자바에서는 **와일드카드(wildcard)** 로 메서드의 매개변수와 반환 타입이 변하는 방식을 지정한다. 이 매커니즘을 **사용처 가변성(use-site variance)** 이라고도 한다.


<br><br>

<h3>6.4.1 서브타입 와일드카드</h3>

---

서로 다른 배열 리스트 사이에서 변환하는 일은 대부분 안전하다. 메서드에서 배열 리스트에 쓰기를 전혀 수행하지 않아 인수로 전달된 배열 리스트를 변경하지 않는다고 했을 때, 와일드 카드로 이 사실을 나타낼 수 있다.

<br>

```java
public static void printNames(ArrayList<? extends Employee> staff) {
	for (int i = 0; i < staff.size(); i++) {
		Employee e = staff.get(i);
		System.out.println(e.getName());
	}
}
```

<br>

와일드카드 타입 ( ? extends Employee )는 미지의 Employee 서브 타입을 가리킨다. 따라서, ArrayList<Employee>나 ArrayList<Manager> 같은 서브타입의 배열리스트로 printNames 메서드를 호출할 수 있다.

ArrayList<? extends Employee> 클래스에 속한 get 메서드 반환 타입은 똑같이 ( ? extends Employee ) 가 된다. 따라서 Employee e = staff.get(i);는 올바른 문장이 된다.

( ? extends Employee를 Empolyee ) 로 변환할 수는 있지만, 그 어떤 것도 ( ? extends Employee ) 로는 변환할 수 없다. 이게 ArrayList<? extends Employee>에서 읽을 수는 있지만, 쓸 수는 없는 이유이다.

<br><br>

<h3>6.4.2 슈퍼타입 와일드카드</h3>

---

와일드카드 타입 ( ? extends Employee ) 는 Employee의 서브타입을 나타낸다. 이 반대로는 ( ? super Employee )로, 슈퍼타입을 나타내는 와일드카드 타입이다. 이 타입은 보통 함수형 객체에서 매개변수로 유용하다. 

<br>

Predicate 인터페이스 T 타입 객체에 특정 프로퍼티가 있는지 검사하는 메서드가 있는데 위 상황을 보여주는 전형적인 예이다.

```java
public interface Predicate<T> {
	boolean test(T arg);
	...
}
```

<br>

```java
// 특정 프로퍼티가 있는 모든 직원의 이름을 호출한다.
public static void printAll(Employee[] staff, Predicate<Employee> filter) {
	for (Employee e : staff)
		if (filter.test(e))
			System.out.println(e.getName());
}
```
<br>
Predicate<Employee> 타입 객체를 인수로 전달해 이 메서드를 호출할 수 있다. 
Predicate는 함수형 인터페이스이므로 다음과 같이 람다 표현식을 전달해도 된다.

<br>

```java
printAll(employee, e -> e.getSalary() > 100000);

// Predicate<Object>를 사용한 예
Predicate<Object> evenLength = e -> e.toString().length() % 2 == 0;
printAll(employee, evenLength);
```

위에서 Predicate 인터페이스는 불변이므로 Predicate<Object>와 Predicate<Employee> 사이에는 아무런 관계가 없다. 이를 해결하기 위해서는 Predicate<? super Employee>를 모두 허용하는 것이다.

<br>

```java
public static void printAll(Employee[] staff, Predicate<? super Employee> filter) {
	for (Employee e : staff)
		if (filter.test(e))
			System.out.println(e.getName());
}
```

위에서 test의 매개변수 타입은 Employee의 슈퍼타입이므로 Employee 객체도 안전하게 전달할 수 있다. 제네릭 함수형 인터페이스를 메서드 매개변수로 받을 때는 super 와일드카드를 사용해야 한다.

<br>

**Note**

<blockquote>

와일드카드에 ‘PECS’라는 약자를 사용하는데, 이는 producer(생산자)에는 extends, consumer(소비자)에는 super를 사용한다는 뜻이다.

</blockquote>

<br><br>

<h3>6.4.3 타입 변수를 이용한 와일드카드</h3>

---

```java
public static <T> void printAll(T[] elements, Predicate<? super > filter)
```

위 메서드는 T타입이나 T의 슈퍼타입 요소용 필터를 매개변수로 받는다.

<br>

다음으로 Collection<E> 인터페이스에 존재하는 메서드를 알아본다. Collection<E> 인터페이스는 E 타입 요소의 컬렉션을 나타낸다.

<br>

```java
public boolean addAll(Collection<? extends E> c)
```

위 메서드는 요소 타입이 E 또는 E의 서브타입인 다른 컬렉션의 모든 요소를 추가할 수 있다.

<br>

Comparable의 타입 매개변수는 compareTo 메서드의 인수 타입을 나타낸다. 따라서 Collections.sort를 다음과 같이 선언해도 괜찮아 보인다.

```java
public static <T extends Comparable<T>> void sort(List<T> list)
```

<br>

하지만, 이는 너무 제한적이다. Employee 클래스가 Comparable<Employee>를 구현하고 직원을 급여로 비교하고, Manager 클래스가 Employee를 확장한다고 하자. 이때, Manager는 Comparable<Manager>가 아니라 Comparable<Employee>를 구현한다는 점에 유의한다. 따라서 Manager는 Comparable<Manager>의 서브타입이 아니라 Comparable<? super Manager>의 서브타입이다.

<br><br>

<h3>6.4.4 경계 없는 와일드카드</h3>

---

아주 일반적인 연산만 수행하는 상황에서는 경계 없는 와일드카드를 사용할 수 있다.

```java
// ArrayList에 null 요소가 들어있는지 검사하는 메서드
public static boolean hasNulls(ArrayList<?> elements) {
	for (Object e : elements) {
		if (e == null) return true;
	}
	return false;
}
```

위에서 ArrayList의 타입 매개변수는 중요하지 않기에 ArrayList<?>를 사용하는 것이 타당하다.

<br><br>

<h3>6.4.5 와일드카드 캡처</h3>

---

와일드카드를 이용해 swap 메서드를 정의해보자.

```java
public static void swap(ArrayList<?> elements, int i, int j) {
	? temp = elements.get(i); // 작동하지 않음.
	elements.set(i, elements.get(j));
	elements.set(j, temp);
}
```

<br>

?를 타입 인수로 사용할 수 있지만, 타입으로는 사용할 수 없다. 하지만 이를 우회해서 해결하는 방법이 있는데, 헬퍼 메서드를 추가하면 된다.

<br>

```java
public static void swap(ArrayList<?> elements, int i, int j) {
	swapHelper(elements, i, j);
}

private static <T> void swapHelper(ArrayList<T> elements, int i, int j) {
	T temp = elements.get(i);
	elements.set(i, elements.get(j));
	elements.set(j, temp);
}
```

위에서는 **와일드카드 캡처(wildcard capture)** 라는 특별한 규칙 덕분에 swapHelper 호출이 유효하다. swapHelper 메서드의 T 타입 매개변수는 와일드카드 타입을 ‘캡처(capture)’한다. swapHelper는 매개변수에 와일드카드가 포함된 메서드가 아니라 제네릭 메서드이므로 T타입 변수로 변수를 선언할 수 있다.

<br>

와일드카드 캡처의 이점 - API 사용자가 제네릭 메서드 대신 이해하기 쉬운 ArrayList<?>를 볼 수 있다는 점.

<br><br>


<h2>6.5 자바 가상 머신에서 보는 제네릭</h2>

---

자바에 제네릭 타입과 메서드를 추가하는 방식을 채택할 때, 설계자들은 가상 머신에서 타입을 ‘지우는’ 방식으로 구현해 기존 버전 클래스와 호환되게 했다. 이는 기존 방식에서 제네릭으로 옮겨갈 수 있게 해서 인기가 많았지만, 문제가 있었다. 호환성 관점에서 만든 타협이 너무 자주 일어난다는 것이다. 이는 아직도 해결되지 못했다.

<br>

<h3>6.5.1 타입 소거</h3>

---

제네릭 타입을 정의하면 해당 타입은 **로(raw) 타입**으로 컴파일된다. raw 타입이란, 제네릭 타입에서 타입이 소거된 타입을 로 타입이라고 한다. 일반 클래스에는 로 타입이라는 개념이 없다. 기존에 살펴봤던 Entry<K, V> 클래스는 다음과 같이 변환된다.

<br>

```java
public class Entry {
	private Object key;
	private Object value;
	...
}
```

K와 V가 모두 Object로 교체되었다.

<br>

```java
public class Entry<K extends Comparable<? super K> & SErializable,
				   V extends Serializable>
// 만일 Entry 클래스를 위처럼 선언했다면,

public class Entry {
	private Comparable key;
	private Serializable value;
	...
}
// 위 클래스로 교체된다.
```

<br><br>

<h3>6.5.2 캐스트 삽입</h3>

---

타입 소거는 위험해 보이지만, 실제로는 안전하다. 하지만 만일 캐스트를 사용했거나, 제네릭과 로 Entry 타입을 섞어서 사용하여 프로그램을 컴파일 할 때, ‘비검사(unchecked)’ 경로를 표시했다고 하자. 이 때는 기존에 명시했던 타입 말고, 서로 다른 타입으로 된 키가 포함될 수도 있다. 따라서 실행 시간에 안전성 검사도 필요하다. 컴파일러는 소거된 타입이 있는 표현식을 읽어 올 때마다 캐스트를 삽입한다.

<br>

```java
Entry<String, Integer> entry = ...;
String key = entry.getKey();

// 타입이 소거된 getKey 메서드는 Object를 반환하므로 컴파일러는 다음 코드를 만들어 낸다.
String key = (String) entry.getKey();
```

<br><br>

<h3>6.5.3 브리지 메서드</h3>

---

메서드 매개변수와 반환 타입을 소거할 때는 가끔 컴파일러가 **브리지 메서드(bridge method)** 를 만들어 내야 한다.

```java
public class WordList extends ArrayList<String> {
	public boolean add(String e) {
		return isBadWord(e) ? false : super.add(e);
	}
}

// 코드 실행부분

WordList words = ...;
ArrayList<String> strings = words; // 슈퍼클래스로 변환하기에 상관없음.
strings.add("C++");
```

strings.add() 호출은 타입이 소거된 ArrayList 클래스의 add(Object) 메서드를 호출한다. 이때 동적 메서드 조회가 일어나서 WordList 객체로 add를 호출하면 ArrayList가 아니라 WordList의 add 메서드가 호출된다.

<br>

컴파일러는 이를 의도하기 위해 WordList 클래스 안에 다음 브리지 메서드를 만들어 놓는다.

```java
public boolean add(Object e) {
	return add((String) e);
}
```

<br>

strings.add(”C++”) 호출은 add(Object) 메서드를 호출하고, 다시 이 메서드는 WordList 클래스의 add(String) 메서드를 호출한다. 반환 타입이 변할 때도 브리지 메서드가 호출될 수 있다.

<br>

```java
public class WordList extends ArrayList<String> {
	public String get(int i) {
		return super.get(i).toLowerCase();
	}
	...
}
```

<br>

WordList 클래스에는 get 메서드가 두개 있다.

```java
String get(int) // WordList에 정의되어 있다.
Object get(int) // ArrayList에 정의된 메서드를 오버라이드한다.
```

<br>

두 번째 메서드는 컴파일러가 만들어 낸 것으로 첫 번째 메서드를 호출한다. 이번에도 컴파일러는 동적 메서드 조회가 일어나게 하려고 브리지 메서드를 만들어 놓는다.

<br>

위 메서드 두개는 매개변수 타입이 같고 반환 타입이 다르다. 자바에서는 이런 메서드 쌍을 구현할 수 없지만, 가상 머신에서는 메서드를 이름, 매개변수 타입, 그리고 반환 타입으로 명시하기에 컴파일러가 이 메서드 쌍을 만들어 낼 수 있다.

<br>

**Note**

<blockquote>

제네릭 타입에만 브리지 메서드를 사용하는 것은 아니다. 공변 반환 타입(covariant return type)을 구현할 때도 브리지 메서드가 사용된다.

</blockquote>