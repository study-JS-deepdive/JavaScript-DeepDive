## 클로저 정의

MDN의 정의 바뀜

‘둘려싸여진 상태의 참조’와 함께 다발로 묶여진 함수의 콤비네이션

내부 함수로부터 외부함수에의 접근권한을 준다.

함수 생성 시점에 언제나 생김

```jsx
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); // 10
  }

  innerFunc();
}

outerFunc();
```

- 함수는 정의된 곳의 스코프를 따름

함수의 [[Environment]] 내부 슬롯에 상위스코프 참조 저장

```jsx
const x = 1;

function foo() {
  const x = 10;

  // 상위 스코프는 함수 정의 환경(위치)에 따라 결정된다.
  // 함수 호출 위치와 상위 스코프는 아무런 관계가 없다.
  bar();
}

// 함수 bar는 자신의 상위 스코프, 즉 전역 렉시컬 환경을 [[Environment]]에 저장하여 기억한다.
function bar() {
  console.log(x);
}

foo(); // 1
bar(); // 1
```

- foo, bar 둘 다 [[Environment]] 에 전역 스코프 참조

클로저 = OER (외부 렉시컬 환경에 대한 참조) + 실행이 종료 되고 나서도 참조가 유지된다

```jsx
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () { console.log(x); }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

- inner는 outer 안에 중첩으로 정의되어 있어서 outer를 참조
- 그런데 3 에서 outer 함수가 종료되고(실행 컨텍스트 스택에서 제거), 4에서 inner 함수만 남았는데도 외부 참조는 [[Environment]] 에서 살아있음
- 상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아님

![image.png](attachment:fc1db3cf-cf28-4d95-9a6b-d509ba0ac063:image.png)

- 상위 스코프를 참조하는 경우에만 Closure(외부) 가 찍힘

### 클로저 활용

클로저는 상태를 안전하게 변경하고 유지하기 위해 사용

→ 특정 함수에게만 상태변경을 허용

```jsx
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가 시킨다.
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

→ num 을 어디서나 접근해서 바꿔버릴 수 있음 (좋지 않은 코드)

```jsx
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가 시킨다.
    return ++num;
  };
}());

console.log(increase()); // 1
console.log(increase()); // 2
```

- 클로저를 활용한 함수
- 즉시 실행후에 종료되니까 num이 다시 0으로 초기화 되지 않음
- let 이 함수 스코프 안에 있으므로 다른데서 접근해서 바꿀 수 없음

```jsx
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num: 0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    }
  };
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

- 객체를 리턴해서 객체 내부의 각 함수가 클로저가 되도록 함

### 캡슐화와 정보 은닉

캡슐화 - 객체의 상태를 나타내는 프로퍼티, 프로퍼티 참조하고 조작하는 동작인 메서드를 하나로 묶음

→ 감출 목적으로 사용하면 정보 은닉

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

- age를 this에 할당하지 않아 인스턴스에서 접근할 수 없게 함
- 그런데, sayHi를 인스턴스 생성시마다 중복생성하고 있다 → 프로토타입으로 넣어야함
- 프로토타입으로 쓸때는 this로 매서드 정의 하는게 아니니까 문제고, 즉시 실행 + 클로저로

```jsx
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.

```

→ 이렇게 다시 참조하는 경우, _age를 같이 공유하고 있어서 (아까 num처럼), age가 그대로 남음

→ 이 현상 해결하는거 #age, # 붙이기

```jsx
class Person {
  // private 필드
  #name;

  constructor(name) {
    this.#name = name;  // 클래스 내부에서는 접근 가능
  }

  getName() {
    return this.#name;  // 내부 메서드에서도 접근 가능
  }
}

const p = new Person("Blake");

console.log(p.getName()); // "Blake"
console.log(p.#name); // 에러
```
