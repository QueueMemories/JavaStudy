2. 기본 타입

# 1. 부호 있는 정수 타입

| 타입 | 저장 공간 | 범위(포함) |
| --- | --- | --- |
| byte | 1바이트 | -128~127 |
| short | 2바이트 | -32,768~32,767 |
| int | 4바이트 | -2,147,483,648~2,147,483,647(2억이넘음) |
| long | 8바이트 | -9,223,372,036,854,775,808~9,223,372,036,854,775,807 |

Integer.MIN_VALUE / .MAX_VALUE  값을 확인할 수 있다.

long 타입으로 충분하지 않을 때는 BigInteger 클래스를 사용한다.

byte, short 타입은 주로 저수준 파일 처리 등 특수한 응용이나 저장 공간이 귀할 때 큰 배열을 만드는 용도로 사용한다.

long 정수 리터럴은 접미어 L을 사용한다.

2진수 값은 접두어 0b

8진수 값은 접두어 0

16진수 값은 접두어 0x 를 사용한다.

- 왜 사용할까? 데이터 타입을 표현하는 방법을 명확하게 하기위해서 사용하고 메모리를 얼마나 차지하는지 확인할 수 있다.


2. 부동소수점 타입
부동소수점 타입은 소수점 부분이 있는 숫자
타입
저장 공간
float
4바이트
double
8바이트
기본은 double타입이며 접미어 D를 선택적으로 붙일수 있고 float타입은 접미어 F를 붙인다.
양의 무한대를 나타내는 Duble.POSITIVE_INFINITY,
음의 무한대를 나타내는 Double.NEGATIVE_INFINITY,
숫자가 아니라는것을 나타내는 Double.NaN등 특별한 부동 소수점값이 있다.
부동소수점 수는 금융 계산에는 적합하지 않다. ex.(2.0 - 1.1)는 0.899999를 출력하고 이러한 오류는 2진수 체계로 나타내기 때문이다. 정밀한 계산이 필요할땐 BigDecimal 클래스를 사용한다.

# 3. char 타입

char타입은 UTF-16문자 인코딩의 ’코드 유닛’을 나타낸다.
| 특수 코드 | 내용 |
| --- | --- |
| \u | 코드 유닛을 16진수로 ex. ‘\u004A’ ; ‘J’ |
| \n | 라인피드(줄 넘김) |
| \r | 캐리지 리턴(출력 위치를 줄 맨 앞으로) |
| \t | 탭 |
| \b | 백스페이스 |
| \’ | ‘ 출력(이스케이프) |
| \\ | \ 출력(이스케이프) |

# 4. boolean 타입

boolean 타입은 false / true 이다. 

0/1 같은 숫자타입이 아니다.