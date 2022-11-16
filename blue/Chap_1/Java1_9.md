# 1-9 기능적 분해
<br>

### 1.9.1 정적 메서드 선언 및 호출

- 메서드를 선언할 때는 반환 값의 타입(아무것도 반환 x : void), 메서드 이름, 매개변수의 타입과 이름을 메서드 헤더(메서드 선언부)에 작성한다.
- 메서드 바디(메서드 구현부)에 구현할 내용을 채워 넣고 반환할 결과가 있다면 return문을 사용한다.
<br>

### 1.9.2 배열 매개변수와 반환 값

- 배열을 메서드에 전달할 수 있고 메서드는 배열에 대한 참조를 받으며, 참조로 전달받은 배열을 변경 하능하다.

```java
public static void swap(int[] values, int i, int j) {
	int temp = values[i];
	values[i] = values[j];
	values[j] = temp;
}

// 메서드는 배열 반환 가능 매개변수로 받은 배열을 변경하지 않고
// 첫 번째 값과 마지막 값으로 구성된 배열 반환
public static int[] firstLast(int[] values) {
	if (values.length == 0) return new int[0];
	else return new int[] { values[0], values[values.length - 1] };
}
```
<br>

### 1.9.3 가변 인수

- 호출 하는 쪽(caller) 에서 인수를 원하는 개수만큼 넘길 수 있게 하는 메서드도 있다.
    
    ⇒ `System.out.printf(”%d”, n);` 
    
        `System.out.printf(”%d %s”, n, “widgets”);`
    
     첫 번째 호출은 인수가 두 개고 두 번째 호출은 인수가 세 개지만 두 호출 모두 같은 메서드를 호출한다.
    
- average 메서드를 정의해서 `average(3, 4.5, -5, 0)` 처럼 인수를 웒는 개수만큼 전달하면서 average를 호출
    
    ⇒ `public static double average(double... values)`
    
    위와 같이 선언한 매개 변수는 double 타입 배열이 된다.
    
- 메서드 호출시 배열 생성, 호출 하는 쪽에서 전달한 인수들로 채워지기 때문에 구현부에서는 배열처럼 사용하면 된다.

```java
public static double average(double... values) {
	double sum = 0;
	for (double v : values) sum += v;
	return values.length == 0 ? 0 : sum / values.length;
}

// 다음과 같이 호출 가능
double avg = average(3, 4.5, -5, 0);

// 인수들이 이미 배열 안에 있으면 풀어 슬 필요가 없다.
double[] scores = { 3, 4.5, -5, 0 };
double avg = average(scores);

// 가변 매개변수는 반드시 메서드의 마지막 매개변수여야 한다.
// 하지만 다른 매개변수를 가변 매개변수를 가변 매개변수 앞에 둘 수 있다.
public static double max(double first, double... rest) }
	double result = first;
	for (double v : rest) result = Math.max(v, result);
	return result;
}
 
```