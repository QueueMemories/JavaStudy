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

<br><br>

<h2>4.2 Object : 보편적 슈퍼클래스</h3>

---

자바에서 모든 클래스는 직 간접적으로 Object 클래스를 확장한다. 클래스에 명시적인 슈퍼클래스가 없으면 암시적으로 Object를 확장한다.

```java
// 만일
public class Employee { ... } 
// 위 코드를 작성했다면 이는
public class Employee extends Object { ... }
// 위 코드와 같다.
```

Object 클래스는 모든 자바 객체에 적용할 수 있는 메서드를 정의한다.

<br>

**Note**

<blockquote>

배열은 클래스다(기본 타입 배열도 클래스). 따라서 배열을 Object 타입의 참조로 변환할 수 있다.

</blockquote>

<br>

**java.lang.Object 클래스의 메서드**

![https://kookyungmin.github.io/image/java_image/java_image_144.png](https://kookyungmin.github.io/image/java_image/java_image_144.png)

<br><br>

<h3>4.2.1 toString 메서드</h3>

---

Object 클래스의 중요한 메서드 하나는 객체의 문자열 표현을 반환하는 toString 메서드이다. 
toString 메서드는 주로 클래스 이름 뒤에 인스턴스 변수 목록을 대괄호([])로 감싸서 나열하는 형식을 따른다.

<br>

Employee 클래스에서 toString을 구현.

```java
public String toString() {
	return getClass().getName() + "[name=" + name + ",salary=" + salary + "]";
}
// 위에서는 "Employee"라는 문자열을 직접 써넣는 대신 getClass().getName()을 호출하여,
// 서브클래스에서도 toString 메서드가 올바르게 작동함을 알 수 있다.
```

<br>

서브클래스에서는 super.toString()을 호출한 후 해당 서브클래스의 인스턴스 변수를 별도의 대괄호 쌍 안에 넣어 추가한다.

```java
public String toString() {
	return super.toString() + "[bonus=" + bouns + "]";
}
```

<br>

객체를 문자열과 연결하면 자바 컴파일러가 해당 객체의 toString 메서드를 자동으로 호출한다.

```java
Point p = new Point(10, 20);
String message = "The current position is " + p ;
// 위에서는 문자열을 p.toString()과 연결한다.
```

<br>

```java
System.out.println(System.out);
```

위 문장을 출력하면 java.io.printStream@2f6684 를 출력한다.

<br>

그 이유는, PrintStream 클래스를 구현한 사람이 toString 메서드를 오버라이드하지 않았기 때문이다.

<br>

**Caution**

<blockquote>

```java
int[] primes = { 2, 3, 5, 6, 11, 13 }
// 위 배열을 문자열로 출력하기 위해서는
primes.toString() // 이 아닌
Arrays.toString(primes) // 문법을 사용해야 한다.
```

int형 배열의 인스턴스인 primes의 메서드를 사용하는 것이 아닌, Arrays 클래스에 있는 toString 메서드의 매개변수로 primes를 넣어 원하는 결과를 얻을 수 있다.

</blockquote>

<br><br>

<h3>4.2.2 equals 메서드</h3>

---

equals 메서드는 한 객체가 다른 객체와 동등한지 검사한다.

equals 메서드는 Object 클래스에 구현되어 있다. equals 메서드를 오버라이드 하고 싶을 때는, 두 객체가 같은 내용을 담고 있을 때, 같다고 보는 **상태 기반 동등성 검사**가 필요할 때이다. 

<br>
ex) String 클래스에서는 두 문자열이 같은 문자로 구성되어 있는지 검사할 때 사용할 수 있도록 equals 메서드를 오버라이드 해놓았다.

<br>

**Caution**

<blockquote>

equals 메서드를 오버라이드할 때마다 이것과 호환되는 hashCode 메서드도 반드시 작성해야 한다.

</blockquote>

<br>

**equals 메서드를 구현할 때, 거쳐야 하는 단계**

<blockquote>

1. 일반적으로 동등한 객체는 동일하며, 이 검사는 비용이 아주 적게 든다.
2. 모든 equals 메서드는 null과 비교하면 false를 반환해야 한다.
3. equals 메서드는 Object. equals를 오버라이드하므로 매개변수의 타입은 Object다. 따라서 인스턴스 변수를 조사할 수 있도록 실제 타입으로 캐스트해야 한다. 캐스트하기 전에 getClass 메서드나 instanceof 연산자로 타입 검사를 수행한다.
4. 마지막으로 인스턴스 변수를 비교한다. 기본 타입은 == 연산자로 비교한다. 하지만 double 값 일 때는 ±∞ 또는 NaN이 염려되므로 Double. equals를 사용한다. 객체일 때는 equals 메서드 의 널 안전 버전인 Objects. equals를 사용한다. 예를 들어 x가 null일 때 x. equals(y)는 예외를 던지지만, Objects. equals(x, y)는 false를 반환한다.

</blockquote>

<br>

**Tip**

<blockquote>

인스턴스 변수가 배열이라면, 정적 메서드 Arrays.equals로 배열의 길이가 같고, 대응하는 배열 요소도 같은지 검사하면 된다.

</blockquote>

<br>

서브클래스에서 equals 메서드를 정의하고자 할 때, 슈퍼클래스의 equals를 먼저 호출하고, 이게 false면 두 객체는 같을 수 없다.

<br><br>

<h3>4.2.3 hashCode 메서드</h3>

---

해시 코드는 객체에서 파생한 정수 값이다. 이는 중복되지 않도록 잘 뒤섞여 있어야 한다.

```java
int hash = 0;
for (int i = 0; i < length(); i++) {
	hash = 31 * hash + charAt(i);
}
// String에서 해시 코드를 계산하는 알고리즘
```

hashCode와 equals 메서드는 반드시 호환되어야 한다. 즉, x.equals(y)면 x.hashCode() == y.hashCode() 여야 한다.

<br>

equals 메서드를 재정의한다면, hashCode 메서드도 재정의해서 equals와 호환되게 해야 한다. 이때는, hashCode 메서드 안에서 인스턴스 변수의 해시 코드를 단순히 결합하면 된다.

```java
public int hashCode() {
	 return Objects.hash(description, price);
}
```

가변 인수 메서드인 Objects.hash는 인수들의 해시 코드를 계산해 결합한다. (널에 안전하다)

<br>

**Caution**

<blockquote>

인터페이스에서는 Object 클래스의 메서드를 재정의하는 기본 메서드를 절대 만들 수 없다. 정의할 수 있다고 가정해도 ‘클래스 우선’ 규칙에 따라 이런 메서드는 절대로 기본 메서드보다 우선할 수 없다.

</blockquote>

<br><br>

<h3>4.2.4 객체 복제</h3>

---

clone 메서드는 오버라이드하기 복잡할 뿐만 아니라, 필요한 경우도 드물어 마땅한 이유가 없다면 오버라이드 하지 않는 것이 좋다.

<br>

**clone 메서드의 목적은 객체의 ‘복제본’을 만드는 것**이다. clone을 사용하면 객체 참조만 배끼는 것이 아닌 값 자체를 복사하므로 두 객체 중 하나의 상태를 변경하더라도 나머지 하나는 변하지 않는다.

<br>

clone메서드는 Object 클래스에서 protected 로 선언되어 있어, 클래스 사용자가 인스턴스를 복제할 수 있게 하려면 반드시 clone 메서드를 오버라이드 해야 한다.

<br>

Object.clone 메서드는 **얕은 복사(shallow copy)** 를 수행한다. 즉, 원본 객체에 있는 모든 인스턴스 변수를 복제된 객체로 단순히 복사한다. 이때, 인스턴스 변수가 기본타입, 또는 불변 객체라면 상관 없지만, 객체를 향한 참조가 저장되어 있다면 원본과 복제본은 이 참조를 공유하게 되어, 한쪽에서 수정한 결과가 다른 쪽에 영향을 끼칠 수 있다. 따라서, 이러한 경우에는 clone 메서드를 오버라이드 하여 **깊은 복사(deep copy)** 를 수행해야 한다.

<br>

일반적으로 클래스를 구현하고자 할 때, 다음 상황을 판단해야 한다.

<blockquote>

1. clone 메서드를 구현하지 않아도 되는가 ?
2. 구현해야 한다면 상속받은 clone 메서드를 사용해도 괜찮은가 ? 
3. 그렇지 않다면 clone 메서드에서 깊은 복사를 수행해야 하는가 ?

</blockquote>

<br>

두번째 경우에는 Cloneable 인터페이스를 구현해야 한다. 이는 아무런 메서드도 없는 인터패이스로 태깅(tagging) 인터페이스 또는 마커(marker) 인터페이스라고 한다. Object.clone 메서드는 얕은 복사를 수행하기에 앞서서 클래스가 Cloneable 인터페이스를 구현했는지 검사하고, 구현하지 않았다면 CloneNotSupportedException 을 던진다. 또한 공개 범위를 protected 에서 public으로 변경하고, 반환 타입을 변경해야 한다.

<br>

세번째 경우에 일단, 얕은 복사를 실행하고, 세부적으로 참조가 복사된 것은 따로 복제하여 각각 직접 대입해준다.

<br><br>

<h2>4.3 열거</h2>

---

```java
public enum Size { SMALL, MEDIUM, LARGE, EXTRA_LARGE };
```

<br>

<h3>4.3.1 열거의 메서드</h3>

---

열거 타입의 인스턴스는 유일하기 때문에, equals 메서드를 굳이 사용하지 않고 ==을 사용하면 된다. 또, toString 메서드를 구현하지 않아도 된다. 열거 객체의 이름을 돌려주는 toString 메서드를 자동으로 제공하기 때문이다. 

<br>

toString의 역은 각 열거 타입에 맞게 만들어지는 정적 메서드 valueOf이다.

```java
Size notMySize = Size.valueOf("SMALL");
```

<br>

위 코드는 notMySize를 Size.SMALL로 설정한다. valueOf 메서드는 지정한 이름에 해당하는 인스턴스가 없다면 예외를 던진다.

```java
Size[] notMySize = Size.values();
```

<br>

또한, 각 열거타입은 정적 메서드 values 를 가진다. 이 메서드는 모든 인스턴스를 선언한 순으로 정렬한 배열을 반환한다.

<br>

**TIP**

<blockquote>

향상된 for 루프에서 열거 타입의 모든 인스턴스를 순회할 때, values 메서드를 사용하면 된다.

```java
for (Size size : Size.values()) {
  System.out.println("size = " + size);
}
```

</blockquote>

<br>

ordinal 메서드는 enum 선언에서 인스턴스의 위치(0부터 시작)를 돌려준다. 

```java
Size.MEDIUM.ordinal()
// enum 타입 Size 클래스에서 MEDIUM이 두번째에 있으므로 위 코드는 1을 반환
```

<br>

모든 E 열거 타입은 자동으로 Comparable<E>를 구현하므로 해당 열거 타입에 나열된 객체만을 상대로 비교할 수 있다.

<br>

**java.lang.Enum<E> 클래스의 메서드**

![https://velog.velcdn.com/images%2Fohzzi%2Fpost%2Fe8a58c0f-9bb2-4828-a156-7820776d42a3%2F20210211_205807.png](https://velog.velcdn.com/images%2Fohzzi%2Fpost%2Fe8a58c0f-9bb2-4828-a156-7820776d42a3%2F20210211_205807.png)

<br><br>

<h3>4.3.2 생성자, 메서드, 필드</h3>

---

열거 타입에는 생성자, 메서드, 필드를 추가할 수 있다.

```java
public enum Size {
    SMALL("S"), MEDIUM("M"), LARGE("L"), EXTRA_LARGE("XL");

    private String abbreviation;

    Size(String abbreviation) {
        this.abbreviation = abbreviation;
    }

    public String getAbbreviation() { return abbreviation; }
}
// 열거의 각 인스턴스는 한 번만 생성된다.
```

<br>

**Note**

<blockquote>

열거의 생성자는 언제나 비공개다. Size처럼 private 제어자를 생략해도 되지만, 열거 타입의 생성자를 public 이나 protected로 선언하면 문법 오류가 일어난다.

</blockquote>

<br><br>

<h3>4.3.3 인스턴스의 바디.</h3>

---

enum 인스턴스 각각에 메서드를 추가할 수 있지만, 열거에 정의된 메서드를 오버라이드하는 것이어야 한다.

```java
public enum Operation {
    ADD("+") {
        public int eval(int arg1, int arg2) { return arg1 + arg2; }
    },
    SUBTRACT("-") {
        public int eval(int arg1, int arg2) { return arg1 - arg2; }
    },
    MULTIPLY("*") {
        public int eval(int arg1, int arg2) { return arg1 * arg2; }
    },
    DIVIDE("/") {
        public int eval(int arg1, int arg2) { return arg1 / arg2; }
    };

    private String symbol;
    Operation(String symbol) { this.symbol = symbol; }
    public String getSymbol() { return symbol; }
    
    public abstract int eval(int arg1, int arg2);
		// 열거에 정의된 메서드.
}
```

<br>

계산기 프로그램의 루프에서는 사용자 입력에 따라 이 값 중 하나를 변수에 설정한 후, eval을 호출한다.

```java
Operation op = ...;
int result = op.eval(first, second);
```

<br>

**Note**

<blockquote>

기술적으로 각 상수는 Operation의 익명 서브클래스에 속한다. 따라서 익명 서브클래스의 바디에 넣을 수 있는 것은 열거 멤버의 바디에도 넣을 수 있다.

</blockquote>

<br><br>

<h3>4.3.4 정적 멤버</h3>

---

열거에 정적 멤버를 넣을 수 있다. 하지만, 생성 순서에 주의해야 하고 열거 상수가 정적 멤버보다 먼저 생성되므로 열거 생성자에서 정적 멤버를 참조할 수 없다.

<br>

따라서, 위 문제를 해결하기 위해 정적 초기화 블록(static initializer)에서 초기화 해야 한다. 이 방식을 사용하면 열거 상수가 생성되고 나면 정적 변수 초기화와 정적 초기화 블록이 평소처럼 위쪽부터 차례로 실행된다.

```java
public enum Modifier {
    PUBLIC, PRIVATE, PROTECTED, STATIC, FINAL, ABSTRACT;
    private int mask;

    static {
        int maskBit = 1;
        for (Modifier m : Modifier.values()) {
            m.mask = maskBit;
            maskBit *= 2; 
        }
    }
    
    public int getMask() {
        return mask;
    }
}
```

<br>

**Note**

<blockquote>

클래스 내부에 열거 타입을 중첩할 수도 있다. 이렇게 중첩된 열거는 암시적으로 정적 중첩 클래스가 된다. 따라서 해당 열거 타입의 메서드에서 감싸는 클래스의 인스턴스 변수를 참조할 수 없다.

</blockquote>

<br><br>

<h3>4.3.5 열거를 기준으로 스위치하기 </h3>

---

switch 문에 열거 상수를 사용할 수 있다.

```java
public static int eval(Operation op, int arg1, int arg2) {
    int result = 0;
    switch (op) {
        case ADD: result = arg1 + arg2; break;
        case SUBTRACT: result = arg1 - arg2; break;
        case MULTIPLY: result = arg1 * arg2; break;
        case DIVIDE: result = arg1 / arg2; break;
    }
    return result;
}
```

switch 문에서는 Operation.ADD가 아니라 ADD를 사용해야 한다.

<br>

**Tip**

<blockquote>

switch 문 밖에서 열거의 인스턴스를 간결한 이름으로 참조하고 싶을 때는 정적 임포트 선언을 사용한다.

```java
import static com.horstMann.corejava.Size.*;
```

위 코드를 작성하면 Size.SMALL 대신 SMALL로 사용이 가능하다.

</blockquote>

<br><br>

<h2>4.4 실행 시간 타입 정보와 리소스</h2>

---

자바는 실행 시간에 객체가 어느 클래스에 속하는지 알아낼 수 있다. 또한, 클래스를 어떻게 로드했는지 알아내서 클래스와 관련되 데이터, 즉 리소스(resource)를 로드할 수도 있다.

<br>

<h3>4.4.1 Class 클래스</h3>

---

어떤 객체의 참조가 저장된 Object 타입 변수가 있는 상태에서 해당 객체 정보를 더 얻고 싶다 가정한다.

<br>

getClass 메서드는 Class 클래스의 객체를 돌려준다.

```java
Class<?> cl = obj.getClass();
```

<br>

위에서 Class 의 객체를 얻고 나면 클래스 이름을 알아낼 수 있다.

```java
System.out.println(cl.getName());
```

<br>

정적 메서드 Class.forName으로 Class 객체를 얻는 방법도 있다.

```java
String className = "java.util.Scanner";
Class<?> cl = Class.forName(className);
// java.util.Scanner 클래스를 기술하는 객체다.
```

<br>

**Caution**

<blockquote>

Class.forName 메서드는 리플렉션을 이용하는 다른 메서드와 마찬가지로 무언가 잘못되면 검사 예외를 던진다.

</blockquote>

<br>

Class.forName 메서드의 용도는 컴파일 시간에는 알려지지 않은 클래스의 Class 객체를 생성하는 것이다. 원하는 클래스를 미리 알고 있다면 Class.forName 대신 클래스 리터럴을 사용하자.


```java
Class<?> cl = java.lang.Scanner.class;
// 이외에도 다른 타입 정보를 얻을 때도 /class 접미어를 사용할 수 있다.
```

이 위에서의 .class 는 그 자체로 클래스를 뜻하는 것은 아니고 Type이라고 생각하는게 더 맞는 표현이다.

<br>

**Caution**

<blockquote>

getName 메서드는 배열 타입에서 이상한 이름을 반환한다. 따랏, 배열에 해당하는 Class 객체를 만들려면 getCanonicalName을 사용해 Class.forName 메서드를 호출해야 한다.

</blockquote>

<br>

가상 머신은 각 타입별로 고유한 Class 객체를 관리하기에, ==연산자를 통해 클래스 객체를 비교할 수 있다.

```java
if (other.getClass() == Employee.class) ...
```

<br><br>

<h3>4.4.2 리소스 로드</h3>

---

Class 클래스는 설정 파일이나 이미지처럼 프로그램에 필요한 리소스를 찾아오는 서비스를 제공한다.

<br>

```java
InputStream stream = MyClass.class.getResourceAsStream("Config.txt");
Scanner in = new Scaaner(stream);
```

클래스 파일이 있는 곳과 같은 디렉터리에 리소스를 넣었을 때, 위 방법으로 리소스 파일에 대응하는 입력 스트림을 열 수 있다.

<br>

**Note**

<blockquote>

일부 레거시 메서드와 javax.swing.ImageIcon  생성자는 URL 객체에서 데이터를 읽는다. 이런 메서드나 생성자를 생성할 때는 지정한 리소스에 대응하는 URL을 반환하는 getResource 메서드를 쓰면 된다.

</blockquote>

<br>

리소스가 서브디렉터리에 있을 경우, 절대 경로 및 상대경로로 지정한다. 또한, 클래스들을 JAR 파일로 패키징할 때, 클래스 파일과 리소스를 함께 넣으면 해당 리소스도 찾아올 수 있다.

<br><br>

<h3>4.4.3 클래스 로더</h3>

---

클래스 파일에는 가상 머신 명령어가 저장된다. 각 클래스 파일은 단일 클래스나 인터페이스에 해당하는 명령어를 담는다. 클래스 파일을 파일 시스템, JAR 파일, 원격 위치에 둘 수 있고, 심지어 메모리에서 동적으로 생성할 수도 있다.

<br>

여기서 클래스 로더(class loader)란, 바이트를 로드해서 가상 머신의 클래스나 인터페이스로 변환하는 역할을 한다.

<br>

가상 머신은 main 메서드가 호출될 메인 클래스부터 시작해 필요할 때, 클래스 파일을 로드한다. 메인 클래스는 java.lang.System이나 java.util.Scanner 같은 클래스를 이용하므로, 이런 클래스도 로드하고, 이것이 의존하는 클래스도 로드한다.

<br>

**자바 프로그램을 실행할 때, 최소한으로 로드되는 클래스**

1. **부트스트랩 클래스 로더(bootstrap class loader)** - 가장 기본적인 자바 라이브러리 클래스를 로드한다. 이는 가상 머신의 일부이다.
2. **플랫폼 클래스 로더(platform class loader)** - 다른 라이브러리 클래스를 로드한다. 부트스트랩 클래스 로더가 로드하는 클래스와 달리 보안 정책으로 플랫폼 클래스 퍼미션을 구성할 수 있다.
3. **시스템 클래스 로더(system class loader)** - 애플리케이션 클래스를 로드한다. 이 로더는 클래스 패스와 모듈 패스에 있는 디렉터리와 JAR 파일에서 클래스를 찾는다.

<br>

**Caution**

<blockquote>

현재 버전의 JDK에서 플랫폼 클래스 로더와 시스템 클래스 로더를 찾기 위해서는 System.getProperty(”java.class.path”)를 사용해서 클래스 패스를 찾아야 한다.

</blockquote>

<br>

독자적으로 URLClassLoader 인스턴스를 생성하여 클래스 패스에 없는 디렉터리나 JAR 파일에서 클래스를 로드할 수도 있다. (플러그인을 로드할 때 이 방법을 사용한다)

```java
URL[] urls = {
	new URL("file:///path/to/directory/"),
	new URL("file:///path/to/jarfile/jar")
};
String className = "com.mycompany.plugins.Entry";
try (URLClassLoader loader = new URLClassLoader(urls)) {
	Class<?> cl = Class.forName(className, true, loader);
	// 이제 여기서 cl의 인스턴스를 생성하면 된다.
}
```

<br>

**Caution**

<blockquote>

위의 forName메서드의 두번째 매개변수는 대상 클래스를 로드한 후, 정적 초기화가 일어남을 보장한다.

</blockquote>

<br>

ClassLoader.loadClass 메서드는 정적 초기화 블록을 실행하지 않으므로 사용하지 않는게 좋다.

<br>

**Note**

<blockquote>

URLClassLoader는 파일 시스템에서 클래스를 로드한다. 다른 곳에서 클래스를 로드하기 위해서는 독자적으로 클래스 로더를 구현해야 한다. 이때, 꼭 구현해야 하는 메서드는 findClass 메서드 하나이다.

</blockquote>

<br><br>

<h3>4.4.4 컨텍스트 클래스 로더</h3>

---

클래스는 다른 클래스에서 사용될 때, 투명하게(아무도 모르게) 로드되기에 대부분은 클래스 로딩 과정을 신경쓰지 않아도 된다.

하지만, 클래스를 동적으로 로드하는 메서드가 있고, 또 다른 클래스 로더로 로드된 클래스에서 이 메서드를 호출하면 문제가 생기는 경우가 있다.

<br>

시스템 클래스 로더가 로드하는 유틸리티 클래스를 만들었고, 반환값이 Object인 createInstance 메소드를 만들었다고 가정 → 플러그인 JAR에서 클래스를 읽어 오는 또 다른 클래스 로더로 플러그인을 로드한다. → 이 플러그인은 Util.createInstance(”com.mycompany.plugins.MyClass”)를 호출해 플러그인 JAR에 들어 있는 클래스의 인스턴스를 생성한다.

<br>

위 경우에서 Util.createInstance는 자체적인 클래스 로더를 사용해 Class.forName을 실행하므로 해당 클래스로 더는 플러그인 JAR을 찾지 못하는데, 이 현상을 **클래스 로더 역전(classloader inversion)**이라고 한다.

<br>

클래스 로더 역전의 해결책은 클래스 로더를 유틸리티 메서드에 전달한 후, 이를 다시 forName 메서드에 전달하는 방법이 있다.

<br>

또 다른 방법은, 현재 스레드의 **컨텍스트 클래스 로더(context class loader)**를 사용하는 것이다. 메인 스레드의 컨텍스트 클래스 로더는 시스템 클래스 로더이고, 새 스레드가 생성될 때, 해당 스레드의 컨텍스트 클래스 로더는 생성하는 쪽 스레드의 컨텍스트 클래스 로더로 설정된다. 즉, 결국 그대로 두면 모든 스레드의 컨텍스트 클래스 로더는 시스템 클래스 로더로 설정된다.

<br>

```java
Thread t = Thread.currentThread();
t.setContextClassLoader(loader);
```

위 처럼 작성하면 유틸리티 메서드는 방금 설정한 컨텍스트 클래스 로더를 불러올 수 있다.

애플리케이션은 플러그인 클래스의 메서드를 호출할 때 컨텍스트 클래스 로더를 플러그인 클래스 로더로 설정해야 한다. 그리고 플러그인 클래스의 메서드를 사용한 후에 이전 설정으로 복원해야 한다.

<br>

**Tip**

<blockquote>

클래스를 이름으로 로드하는 메서드를 작성할 때, 단순히 메서드가 속한 클래스의 클래스 로더를 사용하는 건 좋지 않다. 따라서, 메서드를 호출하는 쪽에서 명시적으로 클래스 로더를 전달하는 방법과 컨텍스트 클래스 로더를 사용하는 방법 중 하나를 선택할 수 있게 하는 것이 좋다.

</blockquote>

<br><br>

<h3>4.4.5 서비스 로더</h3>

---

ServiceLoader 클래스를 이용하면 공통 인터페이스를 준수하는 서비스 구현체를 손쉽게 로드할 수 있다.

<br>

예시로, 서비스의 각 인스턴스에서 제공해야 하는 메서드를 나열한 인터페이스(원한다면 슈퍼클래스도 가능)를 정의한다. (암호화(encryption)를 제공하는 서비스)

<br>

서비스 제공자는 이 서비스를 구현하는(인터페이스를 구현하는) 클래스를 한개 이상 제공한다. 다형성의 원리.

<br>

구현 클래스는 서비스 인터페이스와 같은 패키지에 넣을 필요가 없다. 어느 패키지든 넣을 수 있고, 구현 클래스에는 인수 없는 생성자가 반드시 있어야만 한다.(자바 약속에 의해 super로 상위 클래스의 생성자에 접근하게 될 수도 있기 때문에)

<br>

이후 UTF-8 형식으로 인코딩된 텍스트 파일에 프로바이더 이름을 추가한다. META-INF/services 디렉터리에 만들어야 클래스 로더가 찾을 수 있다.

<br>

이제 프로그램에서 서비스 로더를 초기화한다.

```java
public static ServiceLoader<Cipher> cipherLoader = ServiceLoader.load(Cipher.class);
```

위 과정은 한번만 수행해야 한다. 이제 반복자로 모든 구현체를 반복하고, 적절한 객체를 골라서 서비스를 수행하면 된다.