# 1-7 제어 흐름
<br>

### 1.7.1 분기

- if 문은 괄호 안에 조건이 들어가고, 그 뒤에 한 문장 또는 중괄호로 감싼 문장 묶음이 온다.

```java
if (조건문) {
	// 조건이 맞을 경우 실행
} else if (다른 조건문) {
	// 조건이 맞을 경우 실행
} else {
	// 위의 조건과 맞지 않을 경우 실행
}

```

- case 레이블
    - char, byte, short, int 타입 상수 표현식 사용 가능
    - 문자열 리터럴, 열거값 사용 가능

```java
switch (count) {
	case 0: 
		output = "None";
		break;
	case 1:
		output = "One";
		break;
	case 2:
	case 3:
	case 4:
	case 5:
		output = Integer.toString(count);
		break;
	default:
		output = "Many";
		break;
```
<br>

### 1.7.2 루프

- while, do while, for
- while 루프는 조건 검사 결과에 따라 수행할 작업이 남아있는 동안 바디를 계속 실행
- do while 루프는 조건을 평가하기 전에 루프 바디(구현부)를 실행해야 할 때 사용
- for 루프는 반복 횟수가 고정되어 있을 때 주로 사용 ⇒ for(;;) 무한루프 가능
<br>

### 1.7.3 중단과 계속

- 루프 중간에 탈출하기 위해 break 문 사용
- continue 문은 break문과 유사하지만 루프의 끝이 아닌 현재 루프 반복의 끝으로 건너뜀
<br>

### 1.7.4 지역 변수의 유효 범위

- 지역 변수(local variable)는 메서드의 매개 변수를 포함해 메서드 안에 선언한 변수
- 변수의 유효 범위(scope)는 프로그램에서 해당 변수에 접근할 수 있는 부분이다.
```java
while(...) {
	System.out.println(...);
	String input = sc.next(); // 여기서 input의 유효범위가 시작됨
	...
	// 여기서 input의 유효범위가 끝남
}
```

```java
int next;
do {
	next = generator.nextInt(10);
	count++;
} while (next != target);
// next를 while문 내에 선언시 조건으로 사용 불가능
```

```java
// i 변수를 for 문 밖에서도 사용할 경우
int i;
for (i = 0; !found && i < n; i++) {
	...
}
// for문 밖에서도 i 사용가능
```

```java
int i = 0;
while (...) {
	String i = sc.next(); // 또 다른 변수를 선언하는 것은 오류다.
	...
}
// 유효범위 겹치지 않으면 변수이름이 같아도 재사용 가능한 지역변수
for (int i = 0; i < n / 2; i++) {...}
for (int i = n / 2; i < n; i++) {...} // i 재정의 가능
```