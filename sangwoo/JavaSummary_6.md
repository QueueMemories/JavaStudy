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