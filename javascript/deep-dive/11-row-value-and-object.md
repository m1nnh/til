## 11. 원시 값과 객체의 비교

본 내용은 모던 자바스크립트 Deep Dive 책을 정리한 내용입니다.

### 자바스크립트(javascript) 데이터(data) 타입(type)

- number: 정수/부동소숫점을 통째로 number 데이터 타입이 처리
- boolean: true와 false 두 값을 가질 수 있음
- null: 값이 없음
- undefined: 값이 할당되지 않았음
- symbol: ES6에서 추가된 타입, 유니크한 값
- string: 문자열
- object: 객체 타입
- typeof: 변수의 데이터 타입을 확인하기 위한 문법

### 원시 값(primitive value)

#### 변경 불가능한 값(immutable value)

원시 값(primitive value)는 원시 타입(primitive type)의 값, 즉 원시 값은 변경 불가능한 값(immutable value)이다. 한번 생성된 원시 값은 읽기 전용(read only) 값으로써 변경할 수 없다.

- 변수(variable): 하나의 값을 저장하기 위해 확보한 메모리(memory) 공간 자체 또는 그 메모리 공간을 식별하기 위해 불린 이름
- 값(value): 변수에 저장된 데이터

따라서 변경이 불가능하다는 것은 변수가 아니라 값에 대한 진술이다. 즉 원시 값은 병경 불가능하다는 말은 원시 값 자체를 변경할 수 없다는 것이지 변수 값을 변경할 수 없다는 것은 아니다. 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있다. 변수의 상대 개념은 상수이다. 상수는 재할당이 금지된 변수를 말하며 상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 있다. 단, 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있지만 상수는 단 한 번만 할당이 허용되므로 변수 값을 변경할 수 없다. 따라서 **상수와 변경 불가능한 값을 동일시하면 안된다.**

```
const o = {}; // const 키워드(keyword)를 통해 상수 선언

// const 키워드를 사용해 선언한 변수에 할당한 원시 값은 변경할 수 없다.
// 하지만 const 키워드를 사용해 선언한 변수에 할당한 객체는 변경할 수 있다.
o.a = 1;
console.log(o); // {a: 1}
```

변수가 참조하던 메모리 공간의 주소(address)가 변경된 이유는 변수에 할당된 원시 값이 변경 불가능한 값이기 때문이다. 만약 원시 값이 변경 가능한 값이라면 변수에 새로운 원시 값을 재할당했을 때 변수가 가리키던 메모리 공간의 주소를 바꿀 필요없이 원시 값 자체를 변경하면 그만이다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164417430-1667e458-7d98-4edb-b367-9fb3269925ba.png" alt="memory image1" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

하지만 원시 값은 변경 불가능한 값이기 때문에 값을 직접 변경할 수 없다. 따라서 변수 값을 변경하기 위해 원시 값을 재할당하면 새로운 메모리 공간을 확보하고 재할당한 값을 저장한 후, 변수가 참조하던 메모리 공간의 주소를 변경한가. 값의 이러한 특성을 불변성(immutability)이라 한다.

불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다. 만약 재할당 이외에 원시 값인 변수 값을 변경할 수 있다면 예기치 않게 변수 값이 변경될 수 있다는 것을 의미한다. 이는 값의 변경, 즉 상태 변경을 추적하기 어렵게 만든다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164418672-7ea5bbf9-9e84-460f-8421-3bdccce3d3a3.png" alt="memory image2" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

#### 문자열과 불변성

원시 값인 문자열은 다른 원시 값과 비교할 때 독특한 특징이 잇다. 문자열은 0개 이상의 문자(character)로 이뤄진 집합을 말하며, 1개의 문자는 2바이트의 메모리 공간에 저장된다. 따라서 문자열은 몇 개의 문자로 이뤄졌느냐에 따라 필요한 메모리 공간의 크기가 결정된다.

- number: 1, 1000000 모두 동일한 8바이트가 필요
- string: '1'은 2바이트, '1000000'은 14바이트가 필요

```
var str1 = ''; // 0개의 문자로 이뤄진 문자열
var str2 = 'hello'; // 5개의 문자로 이뤄진 문자열
```

자바스크립트는 개발자의 편의를 위해 원시 타입인 문자열 타입을 제공한다. 자바스크립트의 문자열은 원시 타입이며, 변경 불가능하다. 이것은 문자열이 생성된 이후에는 변경할 수 없음을 의미한다.

```
var str = 'hello';
str = 'world';
```

첫 번째 문이 실행되면 문자열 'hello'가 생성되고 식별자 str은 문자열 'hello'가 저장된 메모리 공간의 첫 번째 메모리 셀(cell) 주소를 가리킨다. 그리고 두 번째 문이 실행되면 이전에 생성된 문자열 'hello'를 수정하는 것이 아니라 새로운 문자열 'world'를 메모리에 생성하고, 식별자 str은 이것을 가리킨다. 이때 문자열 'hello'와 'world'는 모두 메모리에 존재한다. 식별자 str은 문자열 'hello'를 가리키고 있다가 문자열 'world'를 가리키도록 변경되었을 뿐이다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164422370-36e38ece-c701-4e46-8432-b30e784b0f12.png" alt="memory image3" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

이번엔 문자열의 한 문자를 변경하면 string이 원시 값인 것을 알 수 있다. 문자열은 유사 배열 객체이면서 이터러블이므로 배열(array)과 유사하게 각 문자에 접근할 수 있다.

```
var str = 'string';

console.log(str[0]); // s

str[0] = 'S';
console.log(str); // string
```

`str[0] = 'S';`처럼 이미 생성된 문자열의 일부 문자를 변경해도 반영되지 않는다. 문자열은 변경 불가능한 값이기 때문이다. 이처럼 한번 생성된 문자열은 읽기 전용 값으로써 변경할 수 없다. 원시 값은 어떤 일이 있어도 불변한다. 따라서 예기치 못한 변경으로부터 자유롭고, 이는 데이터의 신뢰성을 보장한다.

그러나 변수에 새로운 문자열을 재할당하는 것은 가능하다. 이는 기존 문자열을 변경하는 것이 아니라 새로운 문자열을 새롭게 할당하는 것이기 때문이다.

#### 값에 의한 전달(call by value)

```
var score = 80;
var copy = score;

console.log(score); // 80
console.log(copy); // 80

score = 100;
console.log(score); // 100
console.log(copy); // 80
```

위에 코드의 핵심은 "변수에 변수를 할당했을 때 무엇이 어떻게 전달되는가?"이다. `copy = score;`에서 score는 변수 값 80으로 평가되므로 copy 변수에도 80이 할당될 것이다. 이때 새로운 숫자 값 80이 생성되어 copy 변수에 할당된다. 이때 copy와 score는 80의 값을 갖는다는 점에서는 동일하다. 하지만 score 변수와 copy 변수의 값 80은 다른 메모리 공간에 저장된 별개의 값이다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164424160-97e179f7-bb25-445b-aaf9-3c54415c09e2.png" alt="memory image4" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

이 때 `score = 100;`을 선언하면 다음과 같이 메모리 구조가 변경된다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164425248-fbd2a60a-c6af-4723-aeda-616122aa464f.png" alt="memory image5" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

참고로 값에 의한 전달이라는 용어는 자바스크립트를 위한 용어가 아니다. 엄격하게 표현하면 변수에는 값이 전달되는 것이 아니라 메모리 주소가 전달되기 때문이다. 이는 변수와 같은 식별자는 값이 아니라 메모리 주소를 기억하고 있기 때문이다. 값에 의한 전달은 값을 전달하는 것이 아니라 메모리 주소를 전달한다. 단 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.

중요한 것은 변수에 원시 값을 갖는 변수를 할당하면 변수 할당 시점이든, 두 변수 중 어느 하나의 변수에 값을 재할당하는 시점이든 결국은 두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 값은 변경하더라도 서로 간섭할 수 없다는 것이다.

### 객체(object)

객체는 프로퍼티의 개수가 정해져 있지 않으며, 동적으로 추가되고 삭제할 수 있다. 또한 프로퍼티의 값에도 제약이 없다. 따라서 객체는 원시 값과 같이 확보해야 할 메모리 공간의 크기를 사전에 정해 둘 수 없다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/161945053-1b0b2597-bbae-4078-a9cd-5955663b8206.png" alt="hash table image" width="800" height="450"><br />
  <a href="https://ko.wikipedia.org/wiki/%ED%95%B4%EC%8B%9C_%ED%85%8C%EC%9D%B4%EB%B8%94">사진 출처</a>
</p>

#### 변경 가능한 값(mutable value)

객체(참조) 타입의 값, 즉 객체는 변경 가능한 값(mutable value)이다.

```
var person = {
  name: 'minhyeok'
};
```

원시 값은 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 원시 값에 접근할 수 있었다. 즉 원시 값을 할당한 변수는 원시 값 자체를 값으로 갖는다. 하지만 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값(reference value)에 접근할 수 있다. 참조 값은 생성된 객체가 저장된 메모리 공간의 주소, 그 자체다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164427820-55dfd7c5-27a2-4e72-b6b9-ca52ce6a54bd.png" alt="memory image6" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

원시 값을 할당한 변수를 참조하면 메모리에 저장되어 있는 원시 값에 접근한다. 하지만 객체를 할당한 변수를 참조하면 메모리에 저장되어 있는 참조 값을 통해 실제 객체에 접근한다.

```
// 할당이 이뤄지는 시점에 객체 리터럴이 해석되고, 그 결과 객체가 생성된다.
var person = {
    name: 'minhyeok'
};

// person 변수에 저장되어 있는 참조 값으로 실제 객체에 접근한다.
console.log(person); // {name: "minhyeok"};
```

원시 값은 변경 불가능한 값이므로 원시 값을 갖는 변수의 값을 변경하려면 재할당 외에는 방법이 없다. 하지만 객체는 변경 가능한 값이다. 따라서 객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있다. 즉, 재할당 없이 프로퍼티를 동적으로 추가할 수도 있고 프로퍼티 값을 갱신할 수도 있으며 프로퍼티 자체를 삭제할 수도 있다.

```
var person = {
    name: 'minhyeok'
};

person.name = 'park'; // person['name'] = 'park';
person.address = 'Seoul'; // person['address'] = 'Seoul';

console.log(person); // {name: "park", address: "Seoul"}
```

객체는 변경 가능한 값이므로 메모리에 저장된 객체를 직접 수정할 수 있다. 이때 객체를 할당한 변수에 재할당을 하지 않았으므로 객체를 할당한 변수의 참조 값은 변경되지 않는다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164429377-c84c1750-ba62-4d4d-870a-7851ab10028e.png" alt="memory image7" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

객체를 변경할 때마다 원시 값처럼 이전 값을 복사해서 새롭게 생성한다면 명확하고, 신뢰성이 확보되겠지만 객체는 크기가 매우 클 수도 있고, 원시 값처럼 크기가 일정하지도 않으며, 프로퍼티 값이 객체일 수도 있어서 복사(deep copy)해서 생성하는 비용이 많이 든다. 즉, 메모리의 효율적 소비가 어렵고 성능이 나빠진다.

그래서 메모리를 효율적으로 사용하기 위해 객체를 복사해 생성하는 비용을 절약하여 성능을 향상시키기 위해 객체는 변경 가능한 값으로 설계되어 있다. 그래서 원시 값과는 다르게 여러 개의 식별자가 하나의 객체를 공유할 수 있다는 것이다.

#### 참조에 의한 전달(call by reference)

여러 개의 식별자가 하나의 객체를 공유할 수 있다.

```
var parson = {
  name: 'minhyeok'
};

var copy = person; // 얕은 복사
```

객체를 가리키는 변수를 가른 변수에 할당하면 원본의 참조 값이 복사되어 전달된다. 이를 참조에 의한 전달(call by reference)이라 한다.

<p align="center">
  <img src="https://user-images.githubusercontent.com/78870076/164430908-549a4342-38a7-4193-8bf6-77b3b9738ec5.png" alt="memory image8" width="800" height="450"><br/>
  <a>내가 그린 그림</a>
</p>

```
var parson = {
  name: 'minhyeok'
};

var copy = person;

console.log(copy == person); // true

copy.name = 'park';
copy.address = 'Seoul';

console.log(copy) // {name: "park", address: "Seoul"}
console.log(person) // {name: "park", address: "Seoul"}
```

결국 값에 의한 전달과 참조에 의한 전달은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달하는 면에서 동일하다. 다만 식별자가 기억하는 메모리 공간, 즉 변수에 저장되어 있는 값이 원시 값이냐 참조 값이냐의 차이만 있을 뿐이다. 따라서 자바스크립트에는 참조에 의한 전달은 존재하지 않고, 값에 의한 전달만이 존재한다고 말할 수 있다.

```
var parson1 = {
  name: 'minhyeok'
};

var parson2 = {
  name: 'minhyeok'
};

console.log(person1 === person2); // false
console.log(person1.name === person2.name); // true
```

person1과 person2가 가리키는 객체는 비록 내용은 같지만 다른 메모리에 저장된 별개의 객체다 따라서 전혀 다른 값이기 때문에 false가 나온다. 그러나 프로퍼티 값을 참조하는 person1.name, person2.name은 값으로 평가될 수 있는 표현식이다. 두 표현식 모두 원시 값 'minhyeok'으로 평가된다. 따라서 true가 나온다.
