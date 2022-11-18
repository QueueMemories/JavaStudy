# 1-5 문자열
문자열은 문자의 시퀀스(연속)다.  자바에서는 유니코드 문자라면 무엇이든 문자열에 쓸 수 있다.
<br>

### 1.5.1 문자열 연결

- 문자열 연결시 + 연산자 사용하여 연결 가능

 ⇒  “Hello ” +  “Java” 결과값은 “Hello Java”

```java
int age = 42;
String output = age + “ years”; // output은 “42 years”
```

- 문자열 연결과 덧셈을 섞어서 사용하면 예상하지 못한 결과를 얻을 수 있다.

```java
"Next year, you will be " + age + 1 // 논리오류
"Next year, you will be 421" // 섞어서 사용하여 1을 String으로 인식한 것을 볼 수 있다.
"Next year, you will be " + ( age + 1 ) // 이와 같이 사용해야 43으로 인식 한다.
```

- 여러 문자열을 구분자로 구분해서 결합할 때는 join 메서드 사용

  `String names = String.join(”, “, “peter”, “Paul”, “Mary”);` 

⇒ names를 "Peter, Paul, Mary로 설정함

⇒ 첫 번째 인수는 분리 문자열, 두 번째 인수부터는 결합하고 싶은 문자열을 지정

- 최종 결과만 필요한 상황인데도 수많은 문자열을 연결하는 것은 다소 비효율적이다. 이때 StringBuiler를 사용하면 효율적이다

```java
StringBuilder builer = new StringBulder();
while(문자열이 있으면) {
	builder.append(다음 문자열);
}
String result = builder.toString();
```
<br>

### 1.5.2 부분 문자열

- 문자열 분리 할 때 substring 메서드 사용

```java
String greeting = "Hello, World";
String location = greeting.substring(7, 12); // location 을 "World"로 설정한다.
```

- substring 메서드의 첫 번째 인수는 추출할 부분 문자열 시작위치, 문자열의 위치는 0부터 시작
- 두 번째 인수는 부분 문자열에 포함하지 않을 첫 번째 위치
    - 12 - 7 을 하게되면 추출할 문자열의 길이
- 문자열에서 부분 문자열을 구분자로 분리해서 모두 추출할 경우 split메서드로 이 작업을 수행 하여 부분 문자열의 배열을 반환

```java
String names = "Peter, Paul, Mary";
String[] result = names.split(","); // 문자열 세 개로 구성된 배열 ["Peter", "Paul", "Mary"]
```
<br>

### 1.5.3 문자열 비교

- 두 문자열 같은지 비교할 경우 equals 메서드 사용 ⇒ true or false 반환
- == 연산자는 **메모리에서 동일 객체일 때**만 true를 반환

    ⇒가상 머신에서 각 리터럴 문자열의 인스턴스가 오직 한 개씩만 있음

```java
// 주의
String greeting = "Hello, World";
String location = greeting.substring(7, 12);
location == "World"; // false 반환
```

- 객체가 null인지 검사하려면 == 연산자를 사용해야 한다.

    ⇒ 빈 문자열과 (””) null은 다르다는 점, 빈 문자열은 문자열 길이가 0 이지만 null은 문자열이 아님

- null로 메서드를 호출하면 ‘널포인터 예외’가 일어난다 명시적으로 예외처리하지 않으면 프로그램 종료됨

```java
if ( "World".equals(location) )... // 이 검사는 location이 null 일 때도 제대로 작동
```

- 소문자 구별하지 않고 두 문자열 비교 : equalsIgnoreCase 메서드 사용
- 한 문자열이 또 다른 문자열보다 앞에 오는지(오름차순) 알려줌 : compareTo
- `first.compareTo(second)` 이 호출은 first가 second보다 먼저 오면 음의 정수 반환, 반대면 양의 정수 반환 두 문자열이 같으면 0 반환
<br>

### 1.5.4 숫자와 문자열 사이의 변환

- 정수 ⇒ 문자열 : 정적메서드 `Integer.toString` 을 호출

```java
int n = 42;
//-------------------------------------------
String str = Integer.toString(n); // str "42"로 설정
String str2 = Integer.toString(n, 2); // str2를 "101010"으로 설정
//-------------------------------------------
String str = "101010";
int n = Integer.parseInt(str); // n을 101010으로 설정
int n2 = Integer.parseInt(str, 2); // n2를 42로 설정
```
<br>

### 1.5.5 문자열 API

- 유용한 String메서드

| 메서드 | 목적 |
| --- | --- |
| boolean startsWith(String str)
boolean endsWith(String str)
boolean contains(CharSequence str) | 문자열이 지정한 문자열로 시작, 종료 하거나 지정한 문자열을 포함하는지 검사한다. |
| int indexOf(String str)
int lastIndexOf(String str)
int indexOf(String str, int fromIndex)
int lastIndexOf(String str, int fromIndex) | 전체 문자열이나 formIndex에서 시작하는 부분 문자열을 검색해 str이 처음 또는 마지막으로 나타난 위치를 얻는다. 일치하는 부분을 찾지 못하면 -1 을 반환 |
| String replace(CharSequence oldString, CharSequence newString) | oldString이 나타난 부분을 모두 newString으로 교체한 문자열을 반환 |
| String toUpperCase()
String toLowerCase() | 원본 문자열의 모든 문자가 대문자 또는 소문자로 변환된 문자열 반환 |
| String trim() | 앞뒤 공백을 모두 제거한 문자열을 반환 |