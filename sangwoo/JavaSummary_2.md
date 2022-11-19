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
</blockquote>