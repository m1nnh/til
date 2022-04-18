## 10. 객체 리터럴(object literal)

본 내용은 모던 자바스크립트 Deep Dive 책을 정리한 내용입니다.

### 객체(object)

자바스크립트(javascript)는 객체(object) 기반의 프로그래밍 언어이며, 자바스크립트를 구성하는 거의 '모든 것'이 객체다. 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체다.

원시 타입은 단 하나의 값만 나타내지만 객체 타입(object reference type)은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조(data structure)다. 또한 원시 타입의 값, 즉 원시 값은 변경 불가능한 값(immutable value)이지만 객체 타입의 값, 즉 객체는 변경 가능한 값(mutable value)이다.

객체는 0개 이상의 프로퍼티(property)로 구성된 집합이며, 프로퍼티는 키(key)와 값(value)으로 구성된다.

```
var person = {
    name: 'minhyeok', // 프로퍼티(키: 값)
    age: 27 // 프로퍼티(키: 값)
};
```

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다. 자바스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있다. 따라서 함수도 프로퍼티 값으로 사용할 수 있다. 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드(method)라 부른다.

```
var counter = {
    increase: function() {
        this.num++;
    }
}
```

- 프로퍼티: 객체의 상태를 나타내는 값(data)
- 메서드: 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

### 객체 리터럴에 의한 객체 생성

자바스크립트는 프로토타입 기반 객체지향 언어로써 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원한다.

- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)

객체 리터럴은 중괄호({...}) 내에 0개 이상의 프로퍼티를 정의한다. 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

```
var person = {
    name: 'minhyeok',
    sayHello: function() {
      console.log('hello my name is ${this.name}.');
    }
};

console.log(typeof person); // object
console.log(person); // {name: "minhyeok", sayHewllo: f}
```

객체 리터럴의 중괄호는 코드 블록을 의미하지 않는다. 코드 블록의 닫는 중괄호 뒤에는 세미 콜론을 붙이지 않는다. 객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식이다. 객체를 생성하기 위해 클래스를 먼저 정의하고 new 연산자와 함께 생성자를 호출할 필요가 없다. 숫자 값이나 문자열을 만드는 것과 유사하게 리터럴로 객체를 생성한다. 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수 있고, 객체를 생성한 이후에 프로퍼티를 동적으로 추가할 수도 있다.

### 프로퍼티(property)

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.

```
var person = {
  name: 'minhyeok', // key: name, value: minhyeok
  age: 27 /// key: age, value: 27
};
```

프로퍼티를 나열할 때는 쉼표(,)로 구분한다. 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 괜찮다.

- 프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값: 자바스크립트에서 사용할 수 있는 모든 값

문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다. 이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호([...])로 묶어야 한다.

```
var obj = {};
var key = 'hello';

obj[key] = 'world';

console.log(obj); // {hello: "world"}
```

### 메서드(method)

자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있다. 자바스크립트의 함수는 일급 객체다. 따라서 함수는 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다. 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다. 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.

```
var circle = {
  redius: 5,
  getDiameter: function() {
    return 2 * this.radius;
  }

};

console.log(circloe.getDiameter()); // 10
```

### 프로퍼티 접근

프로퍼티에 접근하는 방법은 다음과 같이 두 가지다.

- 마침표 프로퍼티 접근 연산자(.)를 사용하는 마침표 표기법(dot notation)
- 대괄호 프로퍼티 접근 연산자([...])를 사용하는 대괄호 표기법(bracket notation)

```
var person = {
  name: 'minhyeok'
};

console.log(person.name); // 마침표 포기법 접근, minhyeok
console.log(person['name']); // 대괄호 표기법 접근, minhyeok
```

대괄호 표기법을 사용하는 경우 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 값싼 문자열이어야 한다. 대괄호 프로퍼티 접근 연산자 내에 따옴표를 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석한다.

단, 객체에 존재하지 않는 프로퍼티에 접근하면 `undefined`를 반환한다. 이때 `ReferenceError`이 발생하지 않는 것에 주의해야 한다.

```
var person = {
  name: 'minhyeok'
};

console.log(person.age); // undefined
```

프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름, 즉 자바스크립트에서 사용 가능한 유효한 이름이 아니면 반드시 대괄호 표기법을 사용해야 한다. 단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략할 수 있다. 그 외의 경우 대괄호 내에 들어가는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.

### 프로퍼티 값 갱신

이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```
var person = {
  name: 'minhyeok'
};

person.name = 'park';

console.log(person); // {name: "park"}
```

### 프로퍼티 동적 생성

존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고, 프로퍼티 값이 할당된다.

```
var person = {
  name: 'minhyeok'
};

person.age = 27;

console.log(person); // {name: "minhyeok", age: 27}
```

### 프로퍼티 삭제

`delete` 연산자는 객체의 프로퍼티를 삭제한다. 이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다. 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.

```
var person = {
  name: 'minhyeok'
};

person.age = 27; // 프로퍼티 동적 생성

delete person.age;
delete person.address;

console.log(person); // {name: "minhyeok"}
```

### ES6에서 추가된 객체 리터럴의 확장 기능

ES6에서는 더욱 간편하고 표현력 있는 객체 리터럴의 확장 기능을 제공한다.

#### 프로퍼티 축약 표현

객체 리터럴의 프로퍼티는 프로퍼티 키와 프로퍼티 값으로 구성된다. 프로퍼티 값은 변수에 할당된 값, 즉 식별자 표현식일 수도 있다.

```
var x = 1, y = 2;
var obj = {
  x: x,
  y: y
};

console.log(obj); // {x: 1, y: 2}
```

ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 잇다. 이때 프로퍼티 키는 변수 이름으로 자동 생성된다.

```
let x = 1, y = 2;
const obj = {x, y};

console.log(obj); // {x: 1, y: 2}
```

#### 계산된 프로퍼티 이름

문자열 도는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다. 단, 프로퍼티 키로 사용할 표현식을 대괄호([...])로 묶어야 한다. 이를 계산된 프로퍼티 이름이라 한다.

```
var prefix = 'prop';
var i = 0;

var obj = {};

obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

ES6에서는 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성할 수 있다.

```
const prefix = 'prop';
let i = 0;

const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

#### 메서드 축약 표현

ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다.

```
var obj = {
  name: 'minhyeok',
  sayHi: function() {
    console.log('hi! ' + this.name);
  }
};

obj.sayHi() // hi! minhyeok
```

ES6에서는 메서드를 정의할 때 `function` 키워드(keyword)를 생략한 축약 표현을 사용할 수 잇다.

```
const obj {
  name: 'minhyeok',
  sayHi() {
    console.log('hi! ' + this.name);
  }
};

obj.sayHi(); // hi! minhyeok
```

ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다.
