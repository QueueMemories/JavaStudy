**Program** - 컴퓨터에서 실행하는 명령어의 모음. 특정 목적을 수행하도록 프로그래밍 언어로 작성된 소스를 기계어로 컴파일한 것.

**기계어 (machine language)** - 컴퓨터가 이해할 수 있는 0과 1로 이루어진 이진 코드를 사용.

**소스파일 (source file)** - 프로그래밍 언어로 작성한 파일.

**컴파일 (compile)** - 소스 파일을 컴퓨터가 이해할 수 있는 기계어로 변환하는 과정.

**JDK (Java Development Kit)** - 자바로 프로그램을 개발할 수 있는 실행 환경 (Java Virture Machine)과 개발도구 (컴파일러) 등을 제공.

**환경 변수** - 운영체제가 실행하는 데 필요한 정보를 제공해주는 변수.

<br>

<h2>자바 프로그램 개발 과정</h2>

---

1. 파일 확장명이 .java 인 텍스트 파일 생성 후, 자바 언어로 코드를 작성.
2. 위 과정을 거쳐 만들어진 자바 소수 파일을 컴파일러인 javac 명령어로 컴파일.
3. 컴파일이 성공하면 확장명이 .class 인 바이트 코드 파일이 생성.
4. 이 바이트 코드 파일을 완전한 기계어로 번역해서 사용하려면 java 명령어를 사용해야 함.

<br>

<h2>바이트 코드 파일과 자바 가상 기계</h2>

---

자바 프로그램은 완전한 기계어가 아닌, 중간 코드 즉, 바이트 코드(byte code) 파일(.class)로 구성된다. 따라서 바이트 코드 파일을 운영체제에서 바로 실행하는 것이 아니라, 자바 가상 기계 (JVM) 라는 번역기가 필요하다(JVM은 JDK에 포함되어 있는 software).

<br>

이 중간코드인 바이트 코드로 파일이 구성되기 때문에, 어떠한 운영체제에도 제약받지 않고, 자바 가상 머신에서 해석한다면 코드 수정없이 바로 운영체제에서 사용할 수 있게 된다.

<br>

자바는 java 명령어로 바이트 코드 파일을 실행하면 제일 먼저 main() 메소드를 찾아 main 메소드 블록 내부를 실행한다. 따라서 main() 메소드를 프로그램 실행 **진입점 (entry point)** 이라고 부른다.

<br>

<h2>주석</h2>

---

라인주석 ( // ) - //부터 라인 끝까지 주석으로 처리.

범위주석 ( /* .. */ ) - /*와 */ 사이에 있는 내용은 모두 주석 처리.

도큐먼트주석 (/** .. */) - /** 와 */ 사이에 있는 내용은 모두 주석 처리. 주로 javadoc 명령어로 API 도큐먼트를 생성하는데 사용한다.