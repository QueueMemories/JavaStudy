# 1-6 입력과 출력
<br>

### 1.6.1 입력 읽어오기

- `import java.util.Scanner;` java.util패키지에 있는 Scanner사용
- `Scanner sc = new Scanner(System.in);`
- `nextLine` 메서드는 입력 한줄 읽기
- 정수를 읽을 경우 `nextInt`
- 다른 줄, 단어, 정수, 부동소수점 수 있는지 검사할 때

  ⇒ hasNextLine, hasNext, hasNextInt, hasNextDouble 메서드 사용
<br>

### 1.6.2 포맷 적용 출력

- System.out 객체의 print, println, printf

  ⇒ println은 줄바꿈 , printf 는 숫자의 개수를 제한하여 사용할 경우

- `System.out.printf(”%8.2f”, 1000.0 / 3.0);` 부동소수점 수를 8자리 출력 정밀도 2 자리 출력

  ⇒ 결과는 333.33

- 포맷 적용 출력용 변환 문자
    
    
    | 변환문자 | 목적 |
    | --- | --- |
    | a | 16진 부동 소수점 |
    | b | boolean |
    | h | 해시 코드 |
    | t | 날짜와 시간 ( 이제 사용 안함 ) |
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c683a87-eab4-478c-8597-a90bdef30185/Untitled.png)