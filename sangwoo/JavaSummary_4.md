**상속은 기존 클래스를 기반으로 새로운 클래스를 생성하는 과정이다. 기존 클래스를 상속받으면 메서드를 재사용(또는 상속) 하면서 새 메서드와 필드를 추가할 수 있다.**

<br>

**Note**

<blockquote>

인스턴스 변수와 정적 변수를 총칭해서 필드(field)라고 한다. 그리고 필드, 메서드, 클래스 내부에 있는 중첩 클래스와 인터페이스를  총칭해서 멤버(member)라고 한다.

리플랙션은 실행 중인 프로그램에서 클래스와 멤버 정보를 자세히 찾아내는 프로그래밍 기법이다.

</blockquote>

<br><br>

<h2>4장 핵심 내용</h2>

---

- 서브 클래스는 슈퍼 클래스에서 private이 아닌 메서드를 상속하거나 - 오버라이드 할 수 있다.
super 키워드로 슈퍼클래스의 메서드나 생성자를 호출할 수 있다.
- final 메서드는 오버라이드 할 수 없고, final 클래스는 생성할 수 없다.
- abstract 메서드는 구현이 없고, abstract 클래스의 인스턴스는 생성할 수 없다.
- 슈퍼클래스의 protected 멤버는 서브클래스 메서드에서 접근할 수 있다. 하지만 같은 서브클래스의 객체에 적용할 때만 가능하다. protected 멤버는 패키지 내부에서도 접근할 수 있다.
- 클래스는 모두 Object의 서브클래스다. Object 클래스에는 toString, equals, hashCode, clone 메서드가 있다.
- 열거 타입은 Enum의 서브클래스다. Enum 클래스에는 인스턴스 메서드 toString, compareTo 등과 정적 메서드 valueOf가 있다.
- Class 클래스는 자바 타입과 관련된 정보를 제공한다. 정보를 제공할 수 있는 자바 타입은 클래스, 배열, 인터페이스, 기본 타입, void다.
- 클래스 로더를 사용하면 클래스 파일과 함께 있는 리소스를 로드할 수 있다.
- 클래스 로더를 사용하면 클래스 패스에 지정되지 않은 위치에서 클래스를 로드할 수 있다.
- ServiceLoader 클래스는 서비스 구현체를 찾아 선택하는 메커니즘을 제공한다.
- 리플렉션 라이브러리를 사용하면 프로그램에서 객체의 멤버를 발견하고, 변수에 접근하고, 메서드를 호출할 수 있다.
- 프록시 객체는 임의의 인터페이스를 동적으로 구현하며, 모든 메서드 호출을 핸들러로 전달한다.

<br><br>

<h2>4.1 클래스 확장</h2>

---

회사에 직원과 관리자가 있다고 생각했을 때, 둘다 급여를 받지만, 직원은 받은 만큼 일을 하고 관리자는 목표를 달성했을 때, 추가 금액을 받는다고 하면 이런 상황에서 상속을 사용할 수 있다.

<br><br>

<h3>4.1.1 슈퍼클래스와 서브클래스</h3>

---

기존의 Employee 클래스를 상속받는다고 가정.

```java
public class Manager extends Employee {
	// 추가된 필드
	// 추가된 메서드 또는 슈퍼클래스의 메서드를 오버라이드하는 메서드
}
```

extends 키워드는 기존 클래스에서 파생된 새 클래스를 만든다는 것을 나타낸다.

<br>

슈퍼클래스(super class), 상위 클래스 → 기존 클래스

서브클래스(sub class), 하위 클래스 → 새롭게 생성된 클래스

<br>

위 코드에서는 manager가 서브클래스, Employee가 슈퍼클래스이다.

<br><br>

<h3>4.1.2 서브클래스 메서드 정의와 상속</h3>

---

Manager 클래스에 상여금을 저장하는 인스턴스 변수와 상여금을 설정하는 메서드를 추가한다.

```java
public class Manager extends Employee {
	private double bonus;
	...
	public void setBonus(double bonus) {
		this.bonus = bonus;
	}
}

// Manager의 인스턴스를 만들었다면, setBonus 메서드를 호출 가능하다.
// 또한 Employee의 비공개가 아닌 메서드도 호출할 수 있다. ( 상속(inherited) 받은 것 )

Manager boss = new Manager(...); // 매니저 객체 생성
boss.setBonus(10000); // 생성된 인스턴스로 보너스 설정
boss.raiseSalary(5); // 기존에 있던 메서드 사용가능 (Employee의 인스턴스 메서드)
```

<br><br>

<h3>4.1.3 메서드 오버라이딩</h3>

---

그렇다면, 슈퍼클래스의 인스턴스 메소드와 기능 자체는 같지만  세부사항을 조금 수정해야 할 때는 어떻게 해야 할까.

<br>

이런 경우는 슈퍼 클래스의 메소드를 오버라이드하면 된다.

```java
public class Manager extends Employee {
	...
	public double getSalary() { 
		return super.getSalary() + bonus;
	}
}
// 상위 클래스의 getSalary 메서드를 오버라이드하여 기본 급여와 상여금의 합을 반환하게 하였다.
// 슈퍼 클래스의 메소드를 호출할 때는 super를 사용한다.
```

<br>

서브 클래스의 메서드는 슈퍼 클래스의 비공개 인스턴스 변수에 직접 접근할 수 없기에, getSalary 메서드를 통해 기본 급여를 받아오고 이 급여에 bonus 값을 더한 값을 반환한다.

<br>

**Note**

<blockquote>

super는 this와 다르게 객체 참조가 아니다. 동적 메서드 조회를 우회해 특정 메서드를 호출할 때 사용하는 지시자(directive)이다.

</blockquote>

<br>

오버라이드에서 가장 중요한 점은 곡 슈퍼클래스의 매개변수 타입이 정확하게 일치해야 한다는 것이다.

```java
public boolean worksFor(Employee supervisor);
// 어떤 업무를 보고하는 메서드이다. 
// 이를 오버라이드하게 되면, 관리자는 일반 직원에게 보고하지 않는데도 기존 매개변수를 그대로 가져가야 한다.

public class Manager extends Employee {
	...
	public boolean worksFor(Manager supervisor) {
		...
	}
}
// 하지만 위처럼 매개변수가 다르면 기존 메서드와 완전 다른 새 메서드가 생성된다.
// 이렇게 실행해도 오류가 나지 않지만, 이는 명백한 실수이다.

@Override public boolean worksFor(Employee supervisor);
// 따라서 애너테이션을 붙이면 위 같은 오류가 났을 때, 컴파일러가 오류를 알려준다.
```

<br>

메서드를 오버라이드 할 때, 반환 타입을 서브타입으로 바꿀 수 있다. 이를 **공변 반환 타입(covariant return type)** 이라고 한다.

```java
public Employee getSupervisor();
// 위 코드를 
@Override public Manager getSupervisor();
// 이렇게 오버라이드 할 수 있다.
```

<br>

**Caution**

<blockquote>

메서드를 오버라이드할 때, 서브클래스 메서드는 **적어도 슈퍼클래스 메서드만큼은 접근을 허용**해야 한다. 특히 슈퍼 클래스 메서드를 공개로 선언했다면 서브클래스 메서드도 반드시 공개로 선언해야 한다.

</blockquote>

<br><br>

<h3>4.1.4 서브클래스 생성</h3>

---

서브 클래스는 슈퍼클래스의 인스턴스 변수에 접근할 수 없으므로 슈퍼클래스 생성자를 통해 초기화해야 한다.

<br>

```java
public Manager(String name, double salary) {
	super(name, salary);
	bonus = 0;
}
```

여기서의 super는 name과 salary를 인수로 전달하면서 슈퍼클래스의 생성자를 호출한다.

<br>

**슈퍼클래스 생성자를 호출할 때는 반드시 첫 번째 문장으로 사용해야 한다.**

<br><br>

<h3>4.1.5 슈퍼클래스 할당</h3>

---

서브클래스의 객체를 슈퍼클래스 타입 변수에 할당할 수 있다.

<br>

```java
Manager boss = new Manager(...);
// Manager(서브클래스)의 객체를 만듬(boss)

Employee empl = boss;
// 위에서 만든 객체를 Employee(슈퍼클래스) 변수에 할당함.

empl.getSalary();
// empl의 인스턴스 메소드를 실행하면,
// Employee 타입의 변수에 할당했음에도,
// Manager 클래스의 인스턴스 메소드가 실행됨.
```

위 현상은 동적 메서드 조회(dynamic method lookup)이다.

<br>

메서드를 호출할 때, 가상 머신은 객체의 실제 클래스를 살펴보고, 해당 클래스에 맞는 메서드 버전을 찾아서 실행한다. 이 과정을 **동적 메서드 조회**라고 한다.

<br>

그렇다면, 이렇게 굳이 서브클래스를 슈퍼클래스 타입 변수에 할당하는 이유가 무엇일까.

위 처럼 설계하게 되면, 직원이 관리자든 문지기든 또 다른 Employee 서브 클래스의 인스턴스든 상관없이 모든 직원 객체로 동작하는 코드를 작성할 수 있기 때문이다.

```java
Employee[] staff = new Employee[...];
staff[0] = new Employee(...);
staff[1] = new Manager(...);
staff[2] = new Janitor(...);
...
double sum = 0;
for (Employee empl : staff)
	sum += empl.getSalary();
// 위 코드는 동적 메서드 조회를 이용하여 각 실제 객체에 맞는 메서드를 호출하므로,
// Employee 클래스를 상속받는 모든 서브클래스의 연봉을 더할 수 있게 된다.
```

<br>

**Caution**

<blockquote>

배열에서도 슈퍼클래스에 할당할 수 있다. 이를 기술적인 용어로 표현하면 자바 배열은 공변(covariant)한다고 할 수 있다. 이는 편리한 규칙이지만 타입 오류를 일으킬 가능성이 있어 적절하지 않다.

</blockquote>

<br>

**결론**

<blockquote>

평소에는 슈퍼클래스 타입의 변수에 서브클래스를 담아서 관리. ( 묶어서 하나의 공동체로서 판별하거나 출력 또는 화면에 띄워줄 수 있기에 ) 하지만 각 세부적인 기능은 다르기에, 이를 실행할 때는 오버라이드한 메소드로 출력. 하지만, 이러면 오버라이드 하지 않은 메서드는 찾지 못하기에 아래에서 설명하는 instanceof 메서드를 통해 판별 후, 캐스트를 통해 이러한 오류를 제거해야 한다.

</blockquote>

<br><br>

<h3>4.1.6 캐스트</h3>

---

```java
Employee empl = new Manager(...);
empl.setBonus(10000); // 컴파일 시간오류.
```

위 코드에서 컴파일 시간 오류가 나는 이유는 컴파일러는 코드에서 수신한 타입의 메서드만 호출하는지 검사한다. 하지만, 위에서 empl은 Employee 타입이고 Employee에는 setBonus 메소드가 없기에 위 호출은 실행 시간에 성공한다고 해도 명백한 컴파일 시간 오류이다.

<br>

따라서 아래와 같은 코드를 작성하여 instanceof 연산자와 캐스트로 슈퍼클래스 참조를 서브클래스로 변환하여 슈퍼클래스가 갖고 있지않은 메소드를 서브클래스를 통해 사용할 수 있게 된다.

```java
if (empl instanceof Manager) {
	Manager mgr = (Manager) empl;
	mgr.setBouns(10000);
}
```

<br><br>

<h3>4.1.7 최종 메서드와 최종 클래스</h3>

---

메서드를 final로 선언하면 어느 서브클래스도 해당 메서드를 오버라이드할 수 없다.

<br>

가끔 자신이 만든 클래스의 서브클래스를 만들지 못하게 하고 싶을 때, 클래스를 final 제어자를 사용해야 한다. 자바에는 String, LocalTime, URL 같은 final 클래스의 좋은 예가 많이 있다.

<br><br>

4.1.8 추상 메서드와 추상 클래스

---

인터페이스에서 선언한 메서드를 서브 클래스에서 구현하듯이, 클래스는 구현이 없는 메서드를 선언하여 서브 클래스가 해당 메서드를 구현하도록 강제할 수 있다. 이를 **추상 메서드**라고 하며 추상 메서드가 포함된 클래스를 **추상 클래스**라고 한다.

<br>

추상 메서드와 추상 클래스를 구분하기 위해서 abstract 제어자를 붙어야 한다.

```java
public abstract class Person {
	private String name;  // 인스턴스 변수 생성 가능

	public Person(String name) { this.name = name; }  // 비 추상 메서드
	public final String getName() { return name; }  // 비 추상 메서드

	public abstract int getId(); // 추상 메서드
}
```

<br>

**Note**

<blockquote>

추상 클래스는 인터페이스와 달리 인스턴스 변수와 생성자를 포함할 수 있다.

</blockquote>

<br>

추상 클래스의 인스턴스는 생성할 수 없다.

```java
Person p = new Person("Fred"); // 오류
```

<br>

변수에 구체적인 서브 클래스의 객체 참조를 저장한다면 추상 클래스 타입으로 변수를 선언하는 것은 가능하다.

```java
public class Student extends Person {
	private int id;

	public Student(String name, int id) { 
		super(name); 
		this.id = id; 
	}
	public int getId() { return id; }
}
// 위처럼 구체적인 서브클래스를 선언.

Person p = new Student("Fred", 1729);
// 위 처럼 인스턴스 생성 가능
```

<br><br>

<h3>4.1.9 보호 접근</h3>

---

protected는 아래 두개를 가능하게 한다.

1. 메서드를 서브클래스 전용으로 제한
2. 서브클래스 메서드에서 슈퍼클래스의 인스턴스 변수에 접근

<br>

```java
package com.horstmann.employees;

public class Employee {
	protected double salary;
	...
}
// Employee와 같은 패키에 속한 모든 클래스가 이 필드에 접근할 수 있다.
```

<br>

아래는 다른 패키지에 속한 서브클래스이다.

```java
package com.horstmann.managers;

import com.horstmann.employees.Employee;

public class Manager extends Employee {
	...
	public double getSalary() {
		return salary + bonus; // protected 변수인 salary에 접근할 수 있다.
	}
}
```

Manager 클래스의 메서드는 Manager 객체의 salary 변수만 볼 수 있다. 즉, 다른 Employee 객체의 salary 변수는 볼 수 없다. 이 제한은 보호된 기능에 접근할 목적으로 서브클래스를 만드렁 보호 매커니즘을 속이는 일을 방지할 수 있다.

<br>

한번 보호된(protected) 필드로 만들면 나중에 보호를 해제해도 해당 필드를 사용하는 클래스가 제대로 작동하지 않으므로, 주의해서 사용해야 한다.

<br>

**Caution**

<blockquote>

자바에서 protected는 패키지 수준 접근 권한을 부여하므로 다른 패키지에서 시도하는 접근만 보호한다.

</blockquote>

<br><br>

<h3>4.1.10 익명 서브 클래스</h3>

---

인터페이스를 구현하는 익명 클래스를 만들 수 있는 것처럼, 슈퍼클래스를 확장하는 익명 클래스도 만들 수 있다.

```java
ArrayList<String> names = new ArrayList<String>(100) {
	public void add(int index, String element) {
		super.add(index, element);
	}
}
```

슈퍼 클래스 이름 뒤에 오는 괄호 안의 인수는 슈퍼클래스 생성자에 전달됨(100)

ArrayList<String> 의 익명 서브 클래스를 생성하면서 add메서드를 오버라이드 한 예시이다.

<br>

**이중 중괄호 초기화(double brace initialization)** 라고 하는 기교는 내부 클래스 문법을 약간 특이하게 사용한다.

```java
ArrayList<String> friends = new ArrayList<>();
friends.add("Harry");
friends.add("Sally");
// 위처럼 배열을 생성할 수 있지만, 다시 쓸 일이 없다면 익명으로 만들어도 무관하다.

invite(new ArrayList<String>() {{ add("Harry"); add("Sally"); }})
// 위와 같은 방법으로 같은 결과를 만들 수 있다.
```

바깥쪽 중괄호는 ArrayList<String>의 익명 서브클래스를 만든다. 안쪽 중괄호는 초기화 블록이다.

<br>

하지만, 위 방식은 권장하지 않는다. equals 메서드에서 정상적으로 작동하지 않을 수 있기 때문이다.

<br><br>

<h3>4.1.11 상속과 기본 메서드</h3>

---

클래스를 확장하고 인터페이스를 구현하는 클래스가 있는데, 클래스와 인터페이스에 있는 메서드 이름이 같은 경우를 생각해 보자.

<br>

```java
public interface Named {
    default String getName() { return ""; }
}

public class Person {
	...
	public String getName() { return name; }
}

public class Student extends Person implements Named {
	...
}
```

위와 같은 상황에서는 항상 슈퍼 클래스 구현이 인터페이스 구현보다 우선한다. 따라서 서브 클래스에서 충돌을 해결할 필요는 없다. 그렇더라도, 인터페이스 두 개에서 이름이 같은 기본 메서드를 상속받을 때는 충돌을 해결해야 한다.

<br><br>

<h3>4.1.12 super를 이용한 메서드 표현식</h3>

---

메서드 표현식을 object::instanceMethod 형태로 사용할 수 있다. 이때, 객체 참조 대신 super를 사용할 수 있다.

<br>

**super::instanceMethod**

```java
public class Worker {
    public void work() { 
        for (int i = 0; i < 100; i++) System.out.println("Working"); 
    }
}

public class ConcurrentWorker extends Worker {
	public void work() {
		Thread t = new Thread(super::work);
		t.start();
	}
}
```

위에서 ConcurrentWorker의 work 메서드는 run 메서드에서 슈퍼클래스(Worker)의 work 메서드를 호출하는 Runnable로 스레드를 생성한다.