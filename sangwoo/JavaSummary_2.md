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
<br>

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

<h3>2.2.1 인스턴스 변수</h3>

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

**private의 필요성**

1. 프로그램의 어느 부분이 변수를 변경할 수 있는지 제어할 수 있다.
2. 언제든지 내부 표현을 변경할 수 있다.

<br>
<h3>2.2.2 메서드 헤더</h3>

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