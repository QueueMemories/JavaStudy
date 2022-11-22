<h2>2장 핵심 내용</h2>

- 변경자 메서드는 객체 상태를 변경하지만, 접근자 메서드는 객체 상태를 변경하지 않음.
- 자바에서 변수는 객체가 아니라 객체 참조를 저장함.
- 클래스 선언 안에 인스턴스 변수와 메서드 구현을 선언함.
- 인스턴스 메서드는 객체로 호출하고, 호출된 메서드에서는 이 객체를 this 참조로 접근할 수 있다.
- 생성자는 클래스와 이름이 같다. 클래스 안에 오버로드된 생성자를 여러 개 포함할 수 있다.
- 정적 변수는 어떤 객체에도 속하지 않는다. 정적 메서드는 객체로 호출하지 않는다.
- 클래스는 패키지로 조직화된다. 임포트 선언을 하면 프로그램 안에서 패키지 이름을 쓰지 않아도 된다.
- 클래스 안에 다른 클래스를 선언할 수 있다.
- 내부 클래스는 비정적 중첩 클래스다. 내부 클래스의 인스턴스는 자신을 생성한 바깥쪽 클래스의 객체를 참조한다.
- javadoc 유틸리티는 소스 파일을 처리해 선언된 내용과 프로그래머가 작성한 주석으로 HTML 파일을 만든다.

<h2> 2.1 객체 사용</h2>

---

<blockquote>
시초에는 함수(function)를 호출하는 방식으로 프로그램을 작성했다. 
<br><br>
함수를 호출하면 결과를 반환(return)한다. 이를 이용해 작업 방법을 몰라도 다른 누군가가 작성한 함수를 호출할 수 있다.
<br><br>
객체는 여기에 추가로 자신만의 상태(state)가 있다. 이 상태가 메서드를 해서 얻는 결과에 영향을 준다.
<br><br>
캡슐화의 개념은 다른 사람이 구현한 객체의 메소드를 호출할 때, 내부에서 어떻게 동작하는지 몰라도 되는 원칙이다.
<br><br>
자신의 작업을 공유해 다른 개발자가 사용할 수 있는 객체를 제공하고 싶다면, 클래스(class)를 만들어 객체를 생성하고 사용할 수 있게 만든다.
</blockquote><br>

```java

// 객체를 이용한 현재 달의 달력 표시하기.
public static void main(String[] args) {
	LocalDate date = LocalDate.now().withDayOfMonth(1);
	int month;
	if (args.length >= 2) {
		month = Integer.parseInt(args[0]);
		int year = Integer.parseInt(args[1]);
		date = LocalDate.of(year, month, 1);
	} else {
		month = date.getMonthValue();
	}

	System.out.println(" Mon Tue Wed Thu Fri Sat Sun");
	DayOfWeek weekday = date.getDayOfWeek();
	int value = weekday.getValue(); 
    // 1 = 월요일(Monday), ... 7 = 일요일(Sunday)

	for (int i = 1; i < value; i++)
		System.out.print("    ");
	while (date.getMonthValue() == month) {
		System.out.printf("%4d", date.getDayOfMonth());
		date = date.plusDays(1);
		if (date.getDayOfWeek().getValue() == 1) {
			System.out.println();
		}
	}
	if (date.getDayOfWeek().getValue() != 1) {
		System.out.println();
	}
}
```
<br>
<h3>2.1.1 접근자 메서드와 변경자 메서드</h3>

---


**변경자(mutator)** → 호출되는 객체를 변경하는 메서드<br>
**접근자(accessor)** → 객체를 변경하지 않는 메서드<br>
객체 변경은 위험할 수 있다. 동시 접근의 안정성을 높이기 위해 접근자 메서드만 제공해 **불변(immutable)** 객체로 만든다. <br>
그래도 객체의 변경은 필요하다. 대표적인 예시로 배열 리스트의 add, set 메서드가 있다.

```java
ArrayList<String> friends = new ArrayList<>();
	// friends는 선언만 되고 비어있는 상태.
friends.add("Moon");
	// friends에 "Moon" 문자열 추가. friends의 크기는 1이된다.
friends.set(0, "Na");
	// friends 인덱스 0번에 존재하던 문자열을 Na로 변경
	// set과 add는 객체에 접근에 값을 변경한다. 하지만 이때, 공유 자원으로 쓰이고 있는 자원을
	// 마음대로 사용하다간 데이터 충돌이 날 수도 있기에, 적절한 조치가 필요하다.
```
<br>
<h3>2.1.2 객체 참조</h3>

---

자바의 변수에는 오직 객체 참조(reference)만 담을 수 있다. 실제 객체는 다른 곳에 있고, 참조는 실젝 객체를 찾아내는 구현체 고유의 방법이다.

그렇다면, 객체 참조를 담은 변수를 다른 변수에 할당하게 된다면 어떻게 될까.

```java
ArrayList<String> people = friends;
	// friends는 객체를 참조하고 있었는데, people에 이 friends를 대입한다면
	// people는 friends가 참조하고 있는 객체를 같이 참조하게 된다.
```

위 처럼 코드를 작성하게 되면 friends에서 변경한 내용이 people에서도 동일하게 적용된다. 이처럼 객체를 공유하면 효율적이고 편리하지만, 어떤 참조로든 공유 객체를 변경할 수 있다는 것을 꼭 기억해야 한다.<br>
String, LocalData 처럼 클래스 내부에 변경자 메서드가 없다면, 누구도 객체를 변경할 수 없으므로, 해당 객체에 대한 참조를 얼마든지 공유할 수 있다. (당장 필요 없는 상황일 때는 null을 사용한다.)

<blockquote>
Caution

null로 메서드를 호출하면 NullPointerException이 일어난다(내부 뜻은 NullReferenceException). 이래서 null을 넣기보다 Optional 타입을 넣는 것을 권장한다.
</blockquote><br><br>

<h2>2.2 클래스 구현</h2>

---

<h3>2.2.1 인스턴스 변수</h3>

---

자바에서는 인스턴스 변수(instance variable)로 객체의 상태를 나타낸다.

```java
public class Employee {
	private String name;
	private double salary;
}
// 위는 상태를 나타낸다
```

위 선언은 Employee 클래스의 모든 인스턴스는 name과 salary 변수를 가진다는 의미이다.

인스턴스 변수는 주로 private로 선언하는데, 이는 같은 클래스에 속한 메서드만 변수에 접근할 수 있도록 하는 것이다. 

<br>

**private의 필요성**

1. 프로그램의 어느 부분이 변수를 변경할 수 있는지 제어할 수 있다.
2. 언제든지 내부 표현을 변경할 수 있다.

<br>
<h3>2.2.2 메서드 헤더</h3>

---

메서드를 선언할 때는 메서드 이름, 매개변수의 타입과 이름, 반환 타입을 지정해야 한다.

```java
public void raiseSalary(double byPercent)
// 위 메서드는 double 타입 매개변수를 한 개 받고, 어떤 값도 반환하지 않는다.
// getName 메서드는 시그니처(signature(서명))가 다르다.

public String getName()
// 위 메서드는 매개변수가 없고 String을 반환한다.
```
<blockquote>

**Note**

대부분의 메서드는 public(누구든지 메서드 접근 가능)으로 선언하고, 때로 헬퍼 메서드를 private(같은 클래스에 속한 다른 메서드만 접근 가능)으로 선언한다.
</blockquote>

<br>

<h3>2.2.3 메서드 바디</h3>

---

```java
public class Employee {
	private String name;
	private double salary;

	public void raiseSalary(double byPercent) {
		double raise = salary * byPercent / 100;
		salary += raise;
	}
	// 메서드 바디 작성.

	public String getName() {
		return name;
	}
	// 반환값은 return 사용.
}
// 메서드 선언은 클래스 선언 안에 들어가야 함.
```

<br>

<h3>2.2.4 인스턴스 메서드 호출</h3>

---

```java
fred.raiseSalary(5);
// 인수 5는 raiseSalary의 매개변수인 byPercent를 초기화하는데 사용된다.
public void raiseSalary(double byPercent)
// 위에서 하는 동작은 double byPercent = 5; 이다.

// 이후에 메서드 바디가 실행.
double raise = salary * byPercent / 100;
salary += raise;
// 위에서 salary는 인스턴수 변수로, 
// 메서드 호출에 사용한 인스턴스(fred)에 적용된다는 점을 알고 있어야 한다.
```

<blockquote>
raiseSalary는 인스턴스 메서드(instance method)이고, 이런 메서드는 클래스의 인스턴스에 작동한다. static으로 선언하지 않은 메서드는 모두 인스턴스 메서드이다.

위의 raiseSalary에는 메서드 호출을 받는 객체에 대한 참조, 호출 인수가 전달되고, 메서드 호출을 받는 객체에 대한 참조를 메서드 호출의 수신자(receiver)라고 한다.
</blockquote>

<br>

<h3>2.2.5 this 참조</h3>

---

객체의 메서드를 호출할 때, 해당 객체가 this로 설정된다.

```java
public void raiseSalary(double byPercent) {
	double raise = this.salary * byPercent / 100;
	this.salary += raise;
}
// 위처럼 하면 raise는 지역번수고, salary는 인스턴스 변수인 사실이 명확해진다.

public void setSalary(double salary) {
	this.salary = salary;
}
// 인스턴스 변수와 지역 변수의 이름이 같을 때, salary처럼 한정하지 않은 이름은 지역 변수를 나타내고,
// this.salary는 인스턴스 변수를 나타낸다.
```

원한다면 this를 메서드의 매개변수로도 선언할 수 있지만, 드물게 사용한다.

<br>

<h3>2.2.6 값을 사용한 호출</h3>

---

메서드에 객체를 전달하면 해당 메서드는 객체 참조의 사본을 얻어 이 참조로 매개변수 객체에 접근하거나 매개변수 객체를 변경할 수 있다.

```java
public class EvilManager {
	private Random generator;
	...
	public void giveRandomRaise(Employee e) {
	  double percentage = 10 * generator.nextGaussian();
		e.raiseSalary(percentage);
	}
}

boss.giveRandomRaise(fred);
// fred를 e 매개변수로 복사한다. 
// giveRandomRaise 메서드는 두 참조가 공유하는 객체를 변경한다.
```

<br>

자바에서는 기본 타입 매개변수를 업데이트 하는 메서드를 작성할 수 없다.

```java
public void increaseRandomly(double x) {
	double amount = x * generator.nextDouble();
	x += amount;
}
// 위처럼 double 값을 증가시키는 메서드는 의도한 대로 작동되지 않는다.

boss.increaseRandomly(sales);
// 위처럼 호출하면, sales가 x로 복사된다. 그 후, x를 증가시키지만 sales는 변하지 않는다.
// 이후 매개변수는 유효 범위를 벗어나고 증가 연산은 효력을 잃는다.

public class EvilManager {
	...
	public void replaceWithZombie(Employee e) {
		e = new Employee("", 0);
	}
}

boss.replaceWithZombie(fred);
// 위처럼 호출하면 참조 fred가 e 변수로 복사된다.
// 이후 e는 다른 참조로 설정된다. 메서드가 끝날 때, e는 유효 범위를 벗어나고,
// fred는 어디서도 변경되지 않는다.
```

<br><br>

<h2>2.3 객체 생성</h2>

---

클래스를 완성하기 위해서는 생성자를 구현해야 한다.

<br>

<h3>2.3.1 생성자 구현</h3>

---

생성자 선언 방법은 메서드 선언 방법과 비슷하다. 하지만 생성자는 이름이 클래스와 같고 반환 타입이 없다.

```java
public Employee (String name, double salary) {
	this.name = name;
	this.salary = salary;
}
// 실수로 반환타입(void, int ...) 등을 넣게 되면 메서드를 선언하는게 된다.
```

<br>

<blockquote>
note.

이 생성자는 공개(public) 접근이다. 하지만 비공개(private) 생성자 역시 유용하게 쓰인다.

ex) LocalDate 클래스에는 공개 생성자가 없다. 대신 사용자는 now와 of 등의 ‘팩토리 메서드’를 통해 객체를 얻는다. 이런 경우 비공개 생성자를 호출한다.

생성자는 new 연산자를 사용한 시점에서 실행된다.

```java
new Employee("SangWoo Moon", 500000);
// 위는 Employee 클래스의 객체를 할당한 후, 생성자 바디를 호출한다.
// 생성자 바디는 생성자에 전달된 인수로 인스턴스 변수를 초기화한다.
// new 연산자는 생성된 객체의 참조를 반환하고, 반환받은 참조를 변수에 저장한다.

Employee james = new Employee("SangWoo Moon", 500000);
// or
ArrayList<Employee> staff = new ArrayList<>();
staff.add(new Employee("SangWoo Moon", 500000));
```
</blockquote>

<br>

<h3>2.3.2 오버로딩</h3>

---

생성자는 두 가지 이상의 버전으로 제공할 수 있다. 받는 인수의 타입이 다르거나 개수가 다르면 해당 인수에 맞는 호출이 들어왔을 때 동작한다.

```java
public Employee (String name, double salary) {
	this.name = name;
	this.salary = salary;
}

public Employee (double salary) {
	this.name = "";
	this.salary = salary;
}

Employee james = new Employee("SangWoo Moon", 500000); // 위 생성자를 호출
Employee anonymous = new Employee(40000);              // 아래 생성자를 호출
```

이름은 같은데 매개변수가 다른 메서드가 여러 개 있으면 메서드가 오버로드 된 것이다. 메서드보다 생성자를 오버로드 하는 이유는 생성자는 항상 이름이 클래스 이름과 같아야 하기 때문이다.

<br>

<h3>2.3.3 다른 생성자에서 특정 생성자 호출</h3>

---

비슷한 작업을 수행하는 생성자가 여러 개 있다면 코드 중복을 제거하는 것이 좋다. 공통된 초기화 작업은 대체로 생성자 하나에 몰아넣을 수 있다.

또 다른 생성자에서 어느 한 생성자를 호출하기 위해서는, 호출하는 쪽 생성자 바디의 첫 번째 문장으로만 작성하여야 하고, 생성자 이름이 아닌 this를 통해 호출한다.

```java
public Employee (String name, double salary) {
	this.name = name;
	this.salary = salary;
}

public Employee (double salary) {
	this("", salary); // Employee(String, double)를 호출한다.
  // 코드 중복 제거를 위한 다른 생성자 호출 후, 다른 문장을 이어서 작성한다.
}
// 여기서의 this는 객체 참조가 아니고, 같은 클래스에 속한 다른 생성자를 호출하는 특수 문법이다.
```

<br>

<h3>2.3.4 기본 초기화</h3>

---

생성자 안에서 인스턴스 변수를 명시적으로 설정하지 않으면, 자동으로 변수를 기본 값으로 설정한다.

숫자는 0, bool 값은 false, 객체 참조는 null이 기본 값이다.

```java
public Employee (String name) {
	this.name = name;
}
// Employee 클래스 안에 salary라는 인스턴스가 존재한다면, 
// salary는 0으로 자동 설정된다.

// 여기서 인스턴수 변수는 지역 변수와 완전히 다르다. 따라서 지역 변수는 항상 명시적으로 초기화해야 한다.
```

숫자는 0으로 초기화하면 편하지만, 객체는 null로 초기화 할시 오류가 발생할 확률이 높다.

if (e.getName().equals(”SangWoo Moon”))으로 조건을 검사하면 널 포인터 예외가 일어난다.

<br>

<h3>2.3.5 인스턴스 변수 초기화</h3>

---

```java
public class Employee {
	private String name = "";
	...
}
// 위와 같은 방식으로 인스턴스를 초기화 할 수 있다.
// 이 방법 외에 클래스 선언 안에 임의의 초기화 블록(initialization block)을 넣는 방법도 있다.

public class Employee() {
	private String name = "";
	private int id;
	private double salary;

	{  // 초기화 블록
		Random generator = new Random();
		id = 1 + generator.nextInt(1_000_000);
	}

	public Employee (String name, double salary) {
		...
	}
}
// 초기화 블록은 자주 사용하는 기능은 아니다. 대부분의 개발자는 긴 초기화 코드를 헬퍼 메서드 안에 두고,
// 생성자에서 헬퍼 메서드를 호출한다.
```

인스턴스 변수 초기화와 초기화 블록은 클래스 선언에 나타난 순서로 실행하며, 그 다음에 생성자를 실행한다.

<br>

<h3>2.3.6 최종 인스턴스 변수</h3>

---

인스턴스 변수를 최종(final)으로 선언할 수 있다. 최종으로 선언한 변수는 생성자 실행이 끝나기 전에 초기화해야 한다. 초기화한 후에는 해당 변수를 수정할 수 없다.

```java
public class Employee {
	private final String name;
	...
}
```

<br>
<blockquote>
note.

그렇다면 만일 객체에 final을 쓰게 되면 어떻게 될까.

변경 가능한 객체를 가리키는 참조에 사용하면 final 제어자는 단지 참조 자체가 절대로 변하지 않는다는 사실만 나타낸다. 따라서 객체의 내용을 변경하는 것은 가능하다.
</blockquote>

<br>

```java
public class Person {
	private final ArrayList<Person> friends = new ArrayList<>();
		// 이 배열 리스트에 요소를 추가하는 것은 가능하다.
	...
}
// 메서드에서 friends가 참조하는 배열 리스트를 변경할 수는 있지만,
// 다른 객체로는 절대 교체할 수 없다. 특히 이 참조를 null로 설정하는 것은 불가능하다.
```

<br>

<h3>2.3.7 인수 없는 생성자</h3>

---

인수 없는 생성자는 적절한 기본 값으로 상태를 설정한 객체를 생성한다.

```java
public Employee () {
	name = "";
	salary = 0;
}
// 클래스에 생성자가 없으면 자동으로 아무 작업도 하지 않는 인수 없는 생성자를 받는다.
// 모든 인스턴스 변수는 명시적인 초기화 없이 기본 값(0 or false or null)으로 남는다.
// 즉, 모든 클래스에는 생성자가 적어도 하나는 존재한다.
```
<blockquote>
note.

클래스에 생성자가 이미 있으면 인수 없는 생성자를 자동으로 받지 않는다. 생성자가 있는데도 인수 없는 생성자가 필요하다면 직접 작성해야 한다.
</blockquote>
<blockquote>
note.

객체 소멸시.

C++로 대표되는 일부 프로그래밍 언어에서는 일반적으로 객체가 소멸될 때 해야 할 작업을 지정한다. 자바에는 가비지 컬렉터가 객체를 회수할 때 해당 객체를 ‘마무리 하는’ 매커니즘이 있지만, 마무리 과정이 예측할 수 없는 시간에 일어나므로 이 매커니즘을 사용하지 말아야 한다.
</blockquote>

<br><br>

<h2>2.4 정적 변수와 정적 메서드</h2>

---

static 제어자의 의미를 알아본다.

<br>

<h3>2.4.1 정적 변수</h3>

---

클래스 안에 변수를 static 으로 선언하면 해당 변수는 클래스당 하나만 존재한다. 반면 각 객체에는 자체적인 인스턴스 변수의 사본이 들어 있다.

```java
public class Employee {
	private static int lastId = 0;
	private int id;
	...
	public Employee () {
		lastId++;
		id = lastId;
	}
}
// 모든 Employee 객체는 인스턴스 변수인 id를 각자 보유한다. 하지만 lastId 변수는 오직 하나이다.
// lastId 변수는 클래스의 특정 인스턴스가 아니라 클래스 자체에 속한다.

// 새 Employee 객체를 생성하면 공유된 lastId 변수가 증가하고, 
// 인스턴스 변수 id가 증가한 값으로 설정된다. 즉, 모든 직원 객체가 유일한 id 값을 얻는다.
```

<br>
<blockquote>
caution.

여러 스레드가 Employee 객체를 동시에 생성하면 이 코드는 제대로 작동하지 않는다.
</blockquote>

<br>
<blockquote>
note. 

그렇다면 왜 개별 인스턴스가 아닌 클래스에 속하는 변수를 ‘정적(static)’ 이라고 하는가.

단지 c++에서 유래한 의미 없는 유물이다. 의미를 더 적절하게 드러내는 용어는 ‘클래스 변수’이다.
</blockquote>

<br>

<h3>2.4.2 정적 상수</h3>

---

변경 가능한 정적 변수는 드물지만, 정적 상수(static final 변수)는 아주 일반적이다. 예를 들어 Math 클래스는 다음 정적 상수를 선언하고, 프로그램에서는 이 상수를 Math.PI로 접근한다.

```java
public class Math {
	...
	public static final double PI = 3.14159265358979323846;
	...
}
// static 키워드가 없는 PI는 Math 클래스의 인스턴스 변수가 된다. 
// 즉 PI에 접근하려면 Math 클래스의 객체가 필요하고, 
// 모든 Math 객체는 자체적으로 PI의 사본을 가지고 있어야 한다.

public class Employee {
	private static final Random generator = new Random();
	private int id;
	...
	public Employee() {
		id = 1 + generator.nextInt(1_000_000);
	}
}
// 위는 숫자가 아니라 객체를 담는 정적 final 변수의 예이다.
// 난수가 발생할 때마다 새 난수 발생기를 사용하지 않고, 
// 클래스의 모든 인스턴스에서 난수 발생기를 하나 공유하는 방법을 사용한다.

public class System {
	public static final PrintStream out;
	...
}
// System.out은 System 클래스에 위처럼 선언되어 있다.
```

 <br>
 
 <h3>2.4.3 정적 초기화 블록</h3>

 ---

앞에서는 정적 변수를 선언하면서 초기화했다. 하지만 초기화 작업이 추가로 필요한 경우가 있는데, 이 때는 정적 초기화 블록(static initialization block) 안에 넣으면 된다.

```java
public class CreditCardForm {
	private static final ArrayList<Integer> expirationYear = new ArrayList<>();
	static {
		// 다음 20개 년도를 배열 리스트에 추가한다.
		int year = LocalDate.now().getYear();
		for (int i = year; i <= year + 20; i++) {
			expirationYear.add(i);
		}
	}
	...
}
// 정적 초기화는 클래스를 처음 로드할 때 일어난다. 
// 정적 변수를 명시적으로 다른 값으로 설정하지 않으면 0이나 false 또는 null이 된다.
// 모든 정적 변수 초기화와 정적 초기화 블록은 클래스 선언 안에 나타난 순서로 실행된다.
```

<br>
<blockquote>
약주.

네이티브 메서드란 ?

자바에서 구현하지 않는 메서드이고, 자바 언어의 접근 제어 메커니즘을 우회할 수 있다.

JNI(Java Natice Interface)를 사용해서 만든 메서드이다. JNI는 자바 가상 머신을 구동하는 ‘네이티브’ 운영체제에서 제공하는 API에 접근하는 메커니즘이다. 네이티브 메서드는 C/C++ 언어로 작성한다. 주로 특정 운영체제의 고유의 API를 활용하거나 기존 C/C++ 기본 모듈을 자바 프로그램과 연동하려고 사용한다. 이외에도 빠른 계산이 필요한 수학 라이브러리 등에서 사용한다. 하지만 JNI를 사용하면 자바의 이식성이 떨어질 수 있고, 잘못 작성하면 오히려 순수 자바로 작성한 프로그램보다 느릴 수 있으므로 필요한 경우에만 활용하는 것이 좋다.
</blockquote>

<br>

<h3>2.4.4 정적 메서드</h3>

---

정적 베서드는 객체에 작동하지 않는 메서드이다. ex) Math.pow(x, a)

위 메서드는 작업을 수행할 때 Math 객체를 전혀 사용하지 않고, static 제어자로 선언한다.

```java
public class Math {
	public static double pow(double base, double exponent) {
		...
	}
}
// pow를 인스턴스 메서드로 만들지 않는 이유.
// 자바에서 기본 타입은 클래스가 아니므로 double의 인스턴스 메서드가 될 수 없다.
// Math 클래스의 인스턴스 메서드로 만들 수도 있지만, 
// 그렇다면 pow 메서드를 호출하기 위해 Math 객체를 매번 생성해야 했을 것이다.

// 다른 사람이 만든 클래스에 부가 기능을 제공할 수도 있다..

```

<br>
<blockquote>
note.

객체로도 정적 메서드 호출이 가능하다. 하지만 아무 의미 없는 방법이다.

정적 메소드는 객체에 작동하지 않으므로 인스턴스 변수에 접근할 수 없다. 대신 자신이 속한 클래스의 정적 변수에 접근할 수 있다.

```java
public static RandomNumbers {
	private static Random generator = new Random();
	public static int nextInt(int low, int high) {
		return low + generator.nextInt(high - low + 1);
			// 정적 변수 generator 에 접근할 수 있다.
	}
}
```
</blockquote>

<br>

<h3>2.4.5 팩토리 메서드 </h3>

---

정적 메서드는 팩토리 메서드를 만드는데 사용한다. 펙터리 메서드(factory method)는 클래스의 새 인스턴스를 반환하는 정적 메서드를 의미한다.

```java
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
NumberFormat percentFormatter = NumberFormat.getPercentInstance();
double x = 0.1;
System.out.println(currencyFormatter.format(x)); // $0.10을 출력.
System.out.println(percentFormatter.format(x));  // 10%를 출력.
```


- 생성자를 구별하는 유일한 방법은 생성자의 매개변수 타입이기에 매개변수가 없는 생성자를 두개씩 둘 수 없다. 따라서 위에서는 생성자 대신 팩토리 메서드를 사용했다.

- new NumberFormat(…) 생성자는 NumberFormat을 돌려준다. 하지만 팩토리 메서드는 서브클래스의 객체를 반환할 수 있다.

- 팩토리 메서드를 사용하면 불필요하게 새 객체를 생성하는 대신 공유 객체를 반환할 수도 있다. 예를 들어 Collections.emptyList()를 호출하면 변경할 수 없는 빈 리스트(공유 객체)를 반환한다.