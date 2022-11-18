# 1-8 배열과 배열 리스트
배열은 프로그래밍의 기본 요소로 타입이 같은 아이템(항목)을 모으는데 사용한다. 자바는 언어 수준에서 배열 타입을 포함하며, 필요에 따라 늘어나고 줄어드는 배열을 나타내는 ArrayList 클래스 제공함
<br>

### 1.8.1 배열 다루기

- 정수의 배열 int[], String 객체의 배열 String[]
- 요소 접근 예시는 다음과 같다

```java
names = new String[100]; // 0~99 까지의 index를 가지는 배열
// String[] names = new String[100]; 과 같음
// names[0]...names[99] 로 접근 가능
```

- note
    - C 언어의 배열 선언 문법과 같이 변수 이름 뒤에 []를 두어도 됨
    
      ⇒ `int numbers[];`
    
<br>

### 1.8.2 배열 생성

- new 연산자로 배열을 선언하면 배열을 기본 값으로 채운다.
    - 숫자 타입(char 포함) 의 배열은 0으로 채움
    - boolean의 배열은 false로 채움
    - 객체의 배열은 null 참조로 채움
- 배열 생성시 중괄호로 값을 채울 수 있다.

```java
int[] primes = { 2, 3, 4, 5, 7, 11, 13 };

String[] authors = {
	"James Gosling",
	"Bill Joy",
	"Guy Steele",
	// 여기에 이름을 추가하고 각 이름 뒤에 콤마를 붙이면 됨
}

// 기존 배열에 새 배열 추가할 때 사용하는 방법
primes = new int[] { 11, 12, 13, 14, 15, 17, 21 };
```
<br>

### 1.8.3 배열 리스트

- 배열을 생성하기 위해서는 길이를 알아야 함
- 배열은 한번 생성하면 길이 변경이 절대 불가능
- 자바의 패키지 내에 있는 ArrayList 클래스를 사용하면 변경이 가능해짐
- ArrayList 객체는 내부에서 배열을 관리함
    
    ⇒ 내부적으로 배열이 너무 작아지거나 배열의 공간이 많이 남으면 다른 내부배열을 자동으로 생성해서 원본 배열의 요소를 옮김
    
- 배열과 배열 리스트 문법의 차이
    
    ⇒ 배열은 특수 문법 사용 : 
    
    [] 연산자로 요소 접근, 타입에는 Type[], 생성시에는 new Type[]
    
    ⇒ 배열 리스트는 클래스이므로 일반 인스턴스 생성 문법과 메서드호출 문법 사용함
    

```java
ArrayList<String> friends; // 선언만 하는 문장

// <> : 다이아몬드 문법 => 컴파일러가 변수의 타입에서 타입 매개변수 추론
friends = new ArrayList<>(); // 타입지정 안한상태
					// new ArrayList<String>();
friends.add("Peter"); 
friends.add("Paul");

// 배열 리스트용 초깃값 지정 문법 존재 x 다음과 같이 생성
ArrayList<String> friends = new ArrayList<>(List.of("Peter", "Paul"));
```

- List.of 메서드는 지정한 요소들로 구성된 수정 불가능한 리스트를 반환

```java
// ArrayList의 어느 위치든 요소 추가/제거 가능
friends.remove(1);
friends.add(0, "Paul"); // 인덱스 0 앞에 추가

// 배열 리스트의 요소에 접근 하려면 []문법이 아니라 메서드 호출 사용
// get 메서드는 요소를 읽어오고 set 메서드는 요소를 다른 값으로 교체
String first = friends.get(0);
friends.set(1, "Mary");

// size 메서드는 크기 반환
for (int i = 0; i < friends.size(); i++) {
	System.out.println(friends.get(i));
}
```
<br>

### 1.8.4 기본 타입의 레퍼 클래스

- 제네릭 클래스는 기본타입을 타입 매개변수로 사용할 수 없다.
- ArrayList<int>는 규칙에 어긋나기 때문에 래퍼 클래스를 사용한다.
- Byte, Short, Integer, Long, Float, Double, Character, Boolean 타입 사용

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(42);
int first = nmbers.get(0);
```

- 기본 타입과 대응하는 래퍼 타입 사이의 변환은 자동으로 일어난다.
- 위 코드에서 add를 호출하는 과정중에 오토박싱을 거쳐 값 42를 담은 Integer 객체를 자동으로 만듦
- get 호출은 Integer 객체 반환, int 변수에 할당하기 전에 객체는 내부의 int 값을 돌려주도록 언박싱된다.
- note
    
    기본 타입과 래퍼 타입 사이의 변환은 예외 하나만 제외하고 개발자는 신경 쓸 필요가 없다.
    
    ==, != 연산자는 객체의 내용이 아니라 객체 참조를 비교함
    
    ⇒ 문자열과 같이 equals메서드를 호출해야 한다
    
<br>

### 1.8.5 향상된 for 루프

- 배열의 모든 요소를 방문하는 일은 아주 잦기 때문에 향상된 for 루프라는 단축 기법이 생겨남

```java
// 기존 for 루프
int sum = 0;
for( int i = 0; i < numbers.length; i++) {
	sum += numbers[i];
}

// 향상된 for 루프
int sum = 0;
for (int n : numbers) {
	sum += n;
}
```

- 배열의 인덱스 값이 아니라 요소를 순회하기 때문에 numbers[0], numbers[1] 등을 할당 받음
- 배열 리스트도 향상된 for 루프에 사용 가능

```java
for (String name : friends) {
	System.out.println(name);
}
```
<br>

### 1.8.6 배열과 배열 리스트 복사

- 배열 변수를 또 다른 배열 변수로 복사 가능하지만 두 변수가 같은 배열을 참조하게 된다.
    
    ⇒ deepCopy, shallowCopy 개념
    
    ⇒ Arrays.copyOf 정적 메서드를 사용하면 배열의 사본을 만들 수 있다.
    
- 배열 리스트를 복사 하려면 기존 배열 리스트에서 새 배열 리스트를 생성 해야 한다.
    
    ⇒ `ArrayList<String> copiedFriends = new ArrayList<>(friends);`
    
- 배열을 배열 리스트에 복사할 때 위 생성자를 사용할 수 있다.
- List.of 메서드를 사용해 배열을 불변 리스트로 감싼후 ArrayList를 생성하면 된다.
    
    ```java
    String[] names = …;
    ArrayList<String> friends = new ArrayList<>(List.of(names));
    ```
    
- 배열 리스트를 배열에 복사 할 수 있는데 하위 호환성 때문에 반드시 올바른 타입으로 된 배열을 전달 해야 한다.
    
    ⇒ `String[] names = friends.toArray(new String[0]);`
    
- note
    
    기본 타입 배열과 그에 대응하는 래퍼 클래스의 배열 리스트를 상호 변환하기는 쉽지 않다. int[], ArrayList<Integer>를 상호 변환 하려면 명시적인 루프나 IntStream을 사용해야 한다.( 8장 스트림 참고 )
    
<br>

### 1.8.7 배열 알고리즘

- Arrays와 Collections 클래스는 배열과 배열 리스트에서 자주 사용하는 알고리즘을 구현한다.

```java
Arrays.fill(numbers, 0); // int[] 배열
Collections.fill(friends, ""); // ArrayList<String>

// 배열이나 배열 리스트를 정렬할 때 sort 메서드 사용
Arrays.sort(names);
Collections.sort(friends);
```

- note
    
    배열에는 parallelSort 메서드를 사용할 수 있다. parallelSort 메서드는 대상 배열이 클 때 여러 프로세서로 나누어 작업을 수행함, 하지만 배열 리스트에는 사용할 수 없다.
    
- Arrays.toString 메서드는 배열을 문자열로 표현한 결과를 돌려주어 디버깅용으로 배열을 출력할 때 유용하다고 한다.

```java
System.out.println(Arrays.toString(primes));
	// [ 2, 3, 5, 7, 11, 13 ] 을 출력함

// 배열 리스트의 toString 메서드도 동일한 표현을 반환
String elements = friends.toString();
	// elements 를 "[Peter, Paul, Mary]" 로 설정

// 출력 할 때는 toString을 호출할 필요 없음
System.out.println(friends);
	// friends.toString()을 호출해서 결과를 출력한다.

Collections.reverse(names); // 요소들을 뒤집음
Collections.shuffle(names); // 요소들을 임의로 섞음
```
<br>

### 1.8.8 명령줄 인수

- 모든 자바 프로그램의 main 메서드는 문자열의 배열을 매개변수로 받는다
    
    ⇒ `public static void main(String[] args)`
    
- 프로그램을 실행하면 매개변수가 명령줄(command line) 에서 지정한 인수들로 설정된다.
- intelliJ 에서 run 설정의 edit Configuration의 Program Argument에 java Greeting -g cruel world 를 입력한뒤 다음 코드를 실행
```java
public class Greeting {
	public static void main(String[] args) {
		for (int i = 0; i < args.length; i++) {
			String arg = args[i];
			if (arg.equals("-h")) arg = "Hello";
			else if (arg.equals("-g")) arg = "Goodbye";
			System.out.println(arg);
		}
	}
}

// args[0]은 "java", args[1]은 "Greeting", args[2]는 "Goodbye", args[3]은 "cruel", args[4]는 "Hello"가 된다.
```

- java Greeting -g cruel world
    
    ⇒ “java” 와 “Greeting”은 main메서드에 전달되지 않는다는 점 유의

<br>    

### 1.8.9 다차원 배열

- 자바에는 진정한 다차원 배열이 없다.
- 배열의 배열로 다차원 배열을 구현한다.

```java
int[][] square = {
	{ 16, 3, 2, 13 },
	{ 5, 10, 11, 8 },
	{ 4, 15, 14, 1 }
};
// 기술적으로는 int[]로 구성된 1차원 배열이다.
// 요소 접근시 [][] 처럼 두개 사용해야한다.
```

- 초깃값을 제공하지 않을 대는 반드시 new 연산자를 사용하고 행과 열의 개수를 지정해야 한다.

 ⇒ `int[][] square = new int[4][4]; // 행, 열 순서로 개수를 지정`

- 2차원 배열 순회하려면 행, 열 두개가 필요함

```java
int[][] triangle = new int[n][];

for (int r = 0; r < triangle.length; r++) {
	for (int c = 0; c < triangle[r].length; c++) {
		System.out.printf("%4d", triangle[r][c]);
	}
	System.out.println();
}

// 향상된 for 루프
for (int[] row : triangle) {
	for (int element : row) {
		System.out.printf("%4d", element);
	}
	System.out.println();
}

// 디버깅용으로 2차원 배열의 요소 목록을 출력하려면 다음과 같이 호출
System.out.println(Arrays.deepToString(triangle));
```

- note
    
    2차원 배열 리스트라는 것은 없지만, `ArrayList<ArrayList<Integer>>` 타입 변수를 선언하고 각 행을 일일이 만들 수는 있다.