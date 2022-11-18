# 1-3 변수

### 1.3.1 변수 선언

- 자바는 타입 결함이 강한 언어, 따라서 각 변수에는 해당 타입 값만 저장 할 수 있음
- 변수 선언 시 타입과 이름을 지정 해야 하며, 필요하면 초깃값을 지정 할 수도 있다.
    - int total = 0;
- 타입이 같은 변수 여러 개를 한문장으로 선언 가능 Ex)
    - int total = 0, count = 0; // count 는 초기화 되지 않은 정수
        
        ⇒ 자바 개발자 대부분은 각 변수를 개별로 선언하는 방법을 선호
        
- Random generator = new Random(); // 첫 번째 Random 은 generator 변수의 타입, 두 번째 Random은 이 클래스의 객체를 생성하는 new 표현 식의 일부
<br>

### 1.3.2 변수 이름

- 변수 이름은 반드시 문자로 시작해야 함 ( 메서드와 클래스 이름도 동일 )
    - 문자, 숫자 기호 _, $로 구성 가능
    - $ 는 자동 생성되는 이름 용 이므로 직접 이름 지정 시 사용하지 말아야 한다
    - _ 자체는 유효한 변수 이름이 아님
    - 문자와 숫자는 모든 알파벳으로 구성 가능, 대소문자 구분함
    - 이름에는 공백이나 기호 사용 불가
    - double과 같은 키워드 사용 불가
- 보통 변수와 메서드의 이름은 소문자로 시작하고 클래스 이름은 대문자로 시작하는 관례를 따른다.
- 자바 프로그래머는 낙타 표기법 선호 ⇒ `conuntOfInvalidInputs`처럼 이름이 여러 단어로 구성 될 시 각 단어의 첫 글자를 대문자로 쓰는 것을 의미함

<br>

### 1.3.3 변수 초기화

- 메서드 안에 변수를 선언했다면 해당 변수를 반드시 초기화 한 후 사용해야 한다.
    
    다음 코드 컴파일 시 오류 발생
    
    ```java
    int count;
    count++; // 오류 - count가 초기화되지 않았다.
    ```
    
- 컴파일러는 변수를 사용하기 전에 초기화 했는지 검증한다.
    
    다음 코드 컴파일 시 오류 발생
    
    ```java
    int count;
    if ( total == 0 ) {
    	count = 0;
    } else {
    	count++; // 오류 - count 가 초기화되지 않았다.
    }
    ```
    
- 메서드 안에서는 어느 위치에나 변수를 선언할 수 있고, 자바에서는 변수를 사용하기 직전까지 변수 선언 시기를 늦추는 것을 좋은 방식으로 여긴다.
    
    ```java
    Scanner in = new Scanner (System.in); // 1.6.1 입력 읽어오기 참고
    System.out.println("How old are you?");
    int age = in.nextInt(); // 변수 선언을 변수의 초깃값으로 사용할 값을 얻는 시점에 사용
    ```
    
<br>

### 1.3.4 상수

- final 키워드는 한번 할당하면 변경 불가능한 값에 사용함 ⇒ 다른 언어에서 이 값을 상수라고 한다.

```java
final int DAYS_PER_WEEK = 7; // 상수 이름은 대문자로 선언하는 관례

public class Calendar {
	public static final int DAYS_PER_WEEK = 7; 
	// static 키워드로 상수슬 메서드 외부에 선언가능
  ...
}
```

- final 변수를 처음 사용하기 전에 딱 한번만 초기화 한다면 초기화를 나중으로 미룰 수도 있다.

```java
// 다음은 final 변수의 초기화 규칙을 만족한다.
final int DAYS_IN_FEBRUARY;
if (leapYear) {
	DAYS_IN_FEBRUARY = 29;
} else {
	DAYS_IN_FEBRUARY = 28;
}
```

- 위와 같은 이유 때문에 최종 변수라고 한다. 일단 값을 할당 하면 최종 값이 되어 절대로 변경할 수가 없다.
- 열거 타입은 서로 관련 있는 상수의 집합이 필요할 때도 있다.
```java
public static final int MONDAY = 0;
public static final int TUESDAY = 1;
...

enum Weekday { MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY };
// Weekday 는 Weekday.MONDAY 등을 값으로 가지는 타입이 된다.

// 초기화 방법은 다음과 같다.
Weekday startDay = Weekday.MONDAY;
```