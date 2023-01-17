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

6.1 ~ 6.3 내용이 들어올 공간입니다.

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