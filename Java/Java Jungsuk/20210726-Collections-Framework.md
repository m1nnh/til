## 컬렉션 프레임웍

컬렉션 프레임웍이란 <span style = "background-color:#FAF4C0">데이터 군을 저장하는 클래스들을 표준화한 설계</span>를 뜻한다. 컬렉션은 다수의 데이터, 즉 데이터 그룹을, 프레임웍은 표준화된 프로그래밍 방식을 의미한다. 컬렉션 프레임웍은 컬렉션, 다수의 데이터를 다루는 데 필요한 다양하고 풍부한 클래스들을 제공하기 때문에 프로그래머의 짐을 상당히 덜어 주고 있으며, 또한 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화되어 있기 때문에 사용법을 익히기에도 편리하고 재사용성이 높은 코드를 작성할 수 있다는 장점이 있다.

### 컬렉션 프레임웍의 핵심 인터페이스

컬렉션 프레임웍에서는 컬렉션 데이터 그룹을 크게 3가지 타입이 존재한다고 인식하고 각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이스를 정의하였다. 그리고 인터페이스 `List`와 `Set`의 공통된 부분을 다시 뽑아서 새로운 인터페이스인 `Collection`을 추가로 정의하였다.

- List : 순서가 있는 데이터의 집합, 데이터의 중복을 허용한다.
    - 구현 클래스 : ArrayList, LinkedList, Stack, Vector 등
- Set : 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.
    - 구현 클래스 : HashSet, TreeSet 등
- Map : Key와 Value의 쌍으로 이루어진 데이터의 집합, 순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 밸류는 중복을 허용한다.
    - 구현 클래스 : HashMap, TreeMap, Hashtable, Properties 등

List와 Set의 조상인 Collection 인터페이스에는 다음과 같은 메서드들이 정의되어 있다.

- boolean add(Obecjt o), boolean addAll(Collection c) : 지정된 객체 또는 Collection의 객체들을 Collection에 추가한다.
- void clear() : Collection의 모든 객체를 삭제한다.
- boolean contains(Object o), boolean containsAll(Collection c) : 지정된 객체 도는 Collection의 객체들이 Collection에 포함되어 있는지 확인한다.
- boolean equals(Object o) : 동일한 Collection인지 비교한다.
- boolean remove(Object o) : 지정된 객체를 삭제한다.
- boolean isEmpty() : Collection이 비어있는지 확인한다.
- int size() : Collection에 저장된 객체의 개수를 반환한다.

### List 인터페이스

`List` 인터페이스는 <span style = "background-color:#FAF4C0">중복을 허용</span>하면서 <span style = "background-color:#FAF4C0">저장순서가 유지</span>되는 컬렉션을 구현하는데 사용된다.

<center><img src = "https://user-images.githubusercontent.com/78870076/126935017-1f54dc03-0d7e-46a2-bd6e-8a8e55c62ce8.png"></center>

- void add(int index, Object element), boolean addAll(int index, Collection c) : 지정된 위치에 객체 또는 컬렉션에 포함된 객체들을 추가한다.
- Object get(int index) : 지정된 위치에 있는 객체를 반환한다.
- int indexOf(Object o) : 지정된 객체의 위치를 반환한다. 앞에 last를 붙이면 역방향
- Object remove(int index) : 지정된 위치에 객체를 삭제하고 삭제된 객체를 반환한다.
- Object set(int index, Object element) : 지정된 위치에 객체를 저장한다.
- void sort(Comparator c) : 지정된 비교자로 List를 정렬한다.
- List subList(int fromIndex, int toIndex) : 지정된 범위에 있는 객체를 반환한다.

### Map 인터페이스

`Map` 인터페이스는 `Key`와 `Value`을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는 데 사용된다. 키는 중복될 수 없지만 값은 중복을 허용한다. 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게 된다. 

<center><img src = "https://user-images.githubusercontent.com/78870076/126935345-0376e3bb-1b72-4181-9b58-b6aad1f4c310.png"></center>


- void clear() : Map의 모든 객체를 삭제한다.
- boolean containKey(Object key) : 지정된 key 객체와 일치하는 Map의 key 객체가 있는지 확인한다.
- boolean containValue(Object value) : 지정된 value 객체와 일치하는 Map의 value 객체가 있는지 확인한다.
- boolean equals(Object o) : 동일한 Map인지 비교한다.
- Object get(Object key) : 지정한 key 객체에 대응하는 value 객체를 찾아서 반환한다.

### ArrayList

`ArrayList`는 컬렉션 프레임웍에서 가장 많이 사용되는 컬렉션 클래스일 것이다. 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장된다.

- ArrayList() : 크기가 10인 ArrayList를 생성
- ArrayList(Collection c) : 주어진 컬렉션이 저장된 ArrayList를 생성
- boolean add(Object o) : ArrayList의 마지막에 객체를 추가, 성공하면 true
- void add(int index, Object element) : 지정된 위치에 객체를 저장
- boolean contains(Object o) : 지정된 객체가 ArrayList에 포함되어 있는지 확인
- Object get(int index) : 지정된 위치에 저장된 객체를 반환
- int indexOf(Object o) : 지정된 객체가 저장된 위치를 찾아 반환

### LinkedList

배열은 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리 시간이 가장 빠르다는 장점을 가지고 있지만 다음과 같은 단점도 가지고 있다.

- 크기를 변경할 수 없다.
- 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.

이러한 배열의 단점을 보완하기 위해서 링크드 리스트라는 자료구조가 고안되었다. 배열은 모든 데이터가 연속적으로 존재하지만 링크드 리스트는 <span style = "background-color:#FAF4C0">불연속적으로 존재하는 데이터를 서로 연결한 형태로 구성되어 있다.</span>

<center><img src = "https://user-images.githubusercontent.com/78870076/126937069-96ca29a2-1345-4923-a0b5-8d0a320b63f1.png"></center>

위에 그림에서도 알 수 있듯이 링크드 리스트의 각 요소(Node)들은 자신과 연결된 다음 요소에 대한 참조(주소값)과 데이터로 구성되어 있다.

```
class Node {
    Node next; // 다음 요소의 주소를 저장
    Object obj; // 데이터를 저장
}
```

링크드 리스트에서의 데이터 삭제는 간단하다. 삭제하고자 하는 요소의 이전 요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하기만 하면 된다.  단 하나의 참조만 변경하면 삭제가 이루어지는 것이다. 배열처럼 데이터를 이동하기 위해 복사하는 과정이 없기 때문에 처리속도가 매우 빠르다.

링크드 리스트는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전 요소에 대한 접근은 어렵다. 이 점을 보완한 것이 더블 링크드 리스트이다. 더블 링크드 리스트는 단순히 링크드 리스트에 참조 변수를 하나 더 추가하여 다음 요소에 대한 참조뿐 아니라 이전 요소에 대한 참조가 가능하도록 했을 뿐, 그 외에는 링크드 리스트와 같다.

```
class Node {
    Node next; // 다음 요소의 주소를 저장
    Node previous; // 이전 요소의 주소를 저장
    Ojbect obj; // 데이터를 저장
}
```

### Stack과 Queue

자바에서는 스택에는 `ArrayList`와 같은 배열 기반의 컬렉션 클래스가 적합하지만, 큐는 데이터를 꺼낼 때 항상 첫 번째 저장된 데이터를 삭제하므로 `LinkedList`로 구현하는 것이 더 적합하다.

#### Stack

- boolean empty() : stack이 비어있는지 알려준다.
- Object peek() : Stack의 맨 위에 저장된 객체를 반환, pop()과 달리 Stack에서 객체를 거내지는 않음.
- Object pop() : Stack의 맨 위에 저장된 객체를 꺼낸다.
- Object push() : Stack에 객체를 저장한다.
- int search(Obeject o) : Stack에서 주어진 객체를 찾아서 그 위치를 반환, 못찾으면 -1을 반환(배열과 달리 위치는 0이 아닌 1부터 시작)

#### Queue

- boolean add(Obect o) : 지정된 객체를 Queue에 추가한다.
- Object remove() : Queue에서 객체를 꺼내 반환
- Object element() : 삭제 없이 요소를 읽어온다.
- boolean offer(Object o) : Queue에 객체를 저장
- Object poll() : Queue에서 객체를 꺼내서 반환, 비어있으면 null
- Object peek() : 삭제없이 요소를 읽어 온다. 비어있으면 null을 반환

```
Stack stack = new Stack();
Queue queue = new LinkedList(); // Queue 인터페이스의 구현체인 LinkedList를 사용

stack.push("0");
stack.push("1");
stack.push("2");

queue.offer("0");
queue.offer("1");
queue.offer("2");

// stack.pop() -> 2, 1, 0

// queue.poll() -> 0, 1, 2
```

#### PriorityQueue

Queue 인터페이스의 구현체 중의 하나로, 저장한  순서에 고ㅓㅏㄴ계없이 우선순위가 높은 것부터 꺼내게 된다는 특징이 있다. 그리고 null을 저장할 수 없다.

```
Queue pq = new PriorityQueue();
```

우선순위는 숫자가 작을수록 높은 것이다.

#### Deque

Queue의 변형으로, 한 쪽 끝으로만 추가/삭제할 수 있는 Queue와는 달리, Deque는 양쪽 끝에 추가/삭제가 가능하다.


