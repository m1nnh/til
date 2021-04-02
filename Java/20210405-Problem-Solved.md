## :star: JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가

## [Reference](https://github.com/whiteship/live-study)

## 목표
자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.

## 학습할 것
- JVM이란 무엇인가
- 컴파일 하는 방법
- 실행하는 방법
- 바이트코드란 무엇인가
- JIT 컴파일러란 무엇이며 어떻게 동작하는지
- JVM 구성 요소
- JDK와 JRE의 차이

### I. JVM이란 무엇인가
JVM이란 'Java Virtual Machine'의 약자로 '자바를 실행하기 위한 가상 기계'라고 할 수 있다.

![](https://user-images.githubusercontent.com/78870076/112854578-f5779780-90e8-11eb-80fe-ee3e8ebc4541.png)

Java 애플리케이션은 JVM하고만 상호작용을 하기 때문에 OS와 하드웨어에 독립적이라 **다른 OS에서도 프로그램의 변경없이 실행이 가능하다.**

### II. 컴파일 및 실행 방법
자바로 프로그래밍 하기 위해서는 JDK(Java Development Kit)를 설치해야 한다. JDK를 설치하면 JVM과 Java API 외 필요한 프로그램들이 설치된다.

- javac.exe : 자바 컴파일러, 자바 소스코드를 바이트코드로 컴파일한다.
- java.exe : 자바 인터프리터, 바이트코드를 해석하고 소스코드를 출력한다.
- javap.exe : 역어셈블러, 컴파일된 클래스 파일을 원래 소스로 변환

```
class Hello {
    public static void main(String [] args) {
        System.out.println("Hello World.");
    }
}
```

위에 소스파일에 담긴 파일을 Hello.java라 하자. 이 파일을 javac.exe(컴파일)을 통해 Hello.class 파일을 생성한다. Hello.class 파일을 java.exe(실행)통해 소스코드를 출력한다.

```
$ javac Hello.java
$ java Hello
```

### III. 바이트코드
바이트코드는 JVM이 이해할 수 있는 언어이다. 자바 컴파일러에 의해 소스파일이 바이트코드(.class)로 변환되는데, 이는 컴퓨터가 바로 인식할 수 없다. 

### IV. JIT 컴파일러란 무엇이며 어떻게 동작하는지
JIT는 Just In Time의 약자이다. 본래의 소스코드를 컴파일해서 바이트코드로 변환하고, 자바 인터프리터(java.exe)가 바이트코드를 기계어로 해석하고 실행한다. 이 작업은 비용이 많이 들기 때문에 바이트코드를 하드웨어의 기계어로 바로 변환해주는 JIT컴파일러가 등장했다.
JIT 컴파일러는 같은 코드를 매번 해석하는 대신 자주 쓸만한 코드를 컴파일 해두고 사용한다. 이로써 인터프리터의 느린 실행 속도를 개선해 줄 수 있다. 하지만 초기 실행 속도나 메모리 부분에서 약간의 단점이 있다.

### V. JVM 구성요소
- Class Loader : 자바 컴파일러에 의해 변환된 바이트코드를 메모리에 로드한다.
- Execution Engine : Class Loader가 Runtime Data Areas에 불러온 바이트코드를 실행하는데, 인터프리터 방식과 JIT 방식이 있다.
- Garbage Collector : 메모리를 사용하고 실행이 종료되어 다이상 쓰지 않는 객체를 garbage라고 한다. 가비지 콜렉터는 자동으로 메모리를 관리해준다.
- Runtime Data Areas : 프로그램을 실행하기 위해 운영체제로부터 할당받은 공간으로 5가지 영억으로 나눈다.
    - Method Area : JVM이 시작될 때 생성되어 모든 쓰레드가 공유하는 영역이다.
    - Heap : new 연산자로 생성된 객체를 저장하는 공간으로 가비지 콜렉터의 대상이다.
    - Stack : 쓰레드마다 각각 하나씩 존재하며 지역변수, 매개변수 등 매소드 수행 중 발생하는 임시 데이터를 저장한다.
    - PC Register : 쓰레드가 현재 수행중인 명령어의 주소를 갖는다.
    - Native Method : 자바 외의 다른 언어로 작성된 프로그램을 실행시키기 위한 영역이다.

### VI. JDK와 JRE 차이
JDK는 자바를 **개발**하기 위한 환경으로 JRE를 포함한다. JRE는 Java Runtime Enviroment의 약자로 자바를 **실행**시키기 위한 환경으로 JVM을 포함한다.