## 클린 코드와 리팩토링

### 클린 코드

클린 코드란 읽기 쉬운 코드가 클린 코드이다. 클린 코드의 가장 중요한 요소 중 하나는 가독성이라고 볼 수 있다. 즉, 모든 팀원이 이해하기 쉽도록 작성된 코드이다. 가독성이 중요한 이유는, 일반적으로 기존 코드를 변경하고자 할 때, 해석하는 시간과 수정하는 비율이 10:1이라고 한다. 예를 들면 코드를 변경하기 위해서 걸리는 전체 시간이 10시간이라고 하면, 사전에 코드를 분석하는 시간이 9시간 이상 걸린다는 말로 이해하면 쉬울 것 같다.

### 클린 코드의 주요 원칙 

- S (Simple Responsibility Principle) : 하나의 클래스는 하나의 책임만 가져야 한다.
- O (Open/Closed Principle) : 클래스는 확장에 대하여 열려 있어야 하고, 변경에 대해서는 닫혀 있어야 한다.
- L (Liskov Substitution Principle) : 파생 클래스의 메소드는 기반 클래스의 메소드를 대체하여 사용될 수 있어야 한다.
- I (Interface Segregation Principle) : 클라이언트가 사용 하지 않는 메소드에 의존하지 않아야 한다.
- D (Dependency Inversion Principle) : 추상화된 것은 구체적인 것에 의존하면 안된다. (자주 변경되는 구체적인 것에 의존하지 말고 추상화된 것을 참조)

설계 관점으로 클린 코드를 이야기할 때 가장 많이 화자 되는 것이 위의 `SOLID` 원칙이다. 가독성을 높이기 위해서는 다음과 같이 구현해야 한다.

- 네이밍이 잘 되어야 한다.
- 오류가 없어야 한다.
- 중복이 없어야 한다.
- 의존성을 최대한 줄여야 한다.
- 클래스 혹은 메소드가 한가지 일만 처리해야 한다.

### 리팩토링

리팩토링이란 이미 작성한 소스코드에서 구현된 일련의 행위들을 변경없이, 코드의 가독성과 유지보수성을 높이기 위해 내부 구조를 변경하는 것이다. 다시 말해 기능을 유지하되 읽기 좋고 지속적으로 관리하기 편하게 소스코드를 재작성 하는 것이다. 혼동이 있을 수 있는데, 리팩토링은 가독성과 유지보수성을 목표로하며 성능을 최적화하는 것은 다른 문제이다.

### 리팩토링 이유

- 소프트웨어 설계에서 질적 향상을 위해 리팩토링을 한다. 코드 중복을 제거하고, 수정 용이성 향상
- 소프트웨어 이해도를 향하하기 위해, 가독성 향상을 위해 한다. 이 프로젝트에 다른 사람, 다음 사람, 그리고 그 사람이 나일 수도 있다.
- 버그를 찾는데 도움이 된다.
- 프로그램 개발 속도가 향상된다. 좋은 설계 기반에선, 개발 속도를 단축 할 가능성이 높아진다.

### 리팩토링 언제?

- The Rule Of Three : 유사한 내용이 세번 이상 반복할 때, 리팩토링을 고려
    - 똑같거나 비슷한 내용이 세 번이상 작성되있으면 무조건 하는 것이 아니라 상황에 고려하여 리팩토링을 결정하는 것이다.
- 새로운 기능을 추가할 때
    - 지금 작성된 설계, 소스코드에서 새로운 기능을 추가하기 어려워 보이면, 리팩토링을 해야한다. 이러한 상황인 경우 유지보수성이 덜어지며, 가독성 역시 좋지 않을 수 있다.
- 코드리뷰를 할 때
    - 협업하는 개발팀과 함께 코드리뷰는 질을 높일 수 있다. 그러나 인원이 너무 많은 경우에는 비효율적이므로 소수 인원으로 진행하는 것을 권장한다.

### 클린 코드와 리팩토링의 차이 

리팩토링이 더 큰 의미를 가진 것 같다. 클린 코드는 단순히 가독성을 높이기 위한 작업으로 이루어져 있다면, 리팩토리는 클린 코드를 포함한 유지보수를 위한 코드 개선이 이루어진다. 클린 코드와 같은 부분은 설계부터 잘 이루어져 있는 것이 중요하고, 리팩토링은 결과물이 나온 이후 수정이나 추가 작업이 진행될 때 개선해나가는 것이 올바른 방향이다.