## this

this 키워드

메서드가 자신이 속한 객체의 프로퍼티를 참조하기 위해

자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 함

```jsx
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * circle.radius;
  }
};

console.log(circle.getDiameter()); // 10
```

→ 여기서 속한 객체를 가리키는 circle을 this로.

→ getDiameter()가 호출되어 실행되는 시점에 참조 평가

생성자 함수의 경우, 인스턴스를 생성 하기 전까진 this가 가리키는 것을 알 수 없음

→ 생성 후에 this 가 인스턴스를 가리키게 됨

this 바인딩

- 바인딩: 식별자와 값을 연결
- this 가 가리킬 객체를 바인딩 하는 것

### 함수 호출 방식에 따라 달라지는 this 바인딩

- 기본적으로 this 는 전역 객체(window)를 가리킴

```jsx
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

// 객체의 메서드 내부에 정의되어 있으니 .getName() 앞의 객체에 바인딩
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```

→ 일반 함수 - 함수를 호출할 때 바인딩 (전역 객체 window)

→ 생성자 함수 - 인스턴스 생성 할 때 바인딩

| 함수 호출 방식 | this 바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 미래에 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | 메스에 첫번째 인수로 전달한 객체 |

### 렉시컬 스코프와 this 바인딩

렉시컬 스코프 - 함수 정의가 평가되어 함수 객체가 생성되는 시점에 스코프 결정

this 바인딩 - 함수 호출 시점에 결정

### 1. 일반 함수 호출

- 전역 객체가 바인딩 되는데, 사실 객체를 생성하지 않는 일반 함수에서 this 는 의미 없음
- ‘use strict’ strict mode 에서는 undefined
- 중첩 함수, 콜백 함수 모두 전역 객체 바인딩

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();

```

→ 근데 헬퍼 함수나 콜백 함수의 this 에 전역객체가 바인딩되면 문제 있잖아. → 

`Function.prototype.apply/call/bind` 써서 명시적으로 바인딩

화살표 함수로도 할 수 있는데, 화살표 함수는 자기 this를 갖지 않아서 자동으로 상위의 this에 연결

### 2. 메서드 호출

- .(마침표) 앞에 기술한 객체가 바인딩
- 소유가 아닌 호출한 객체에 바인딩

위 함수 호출과 다른건, .getName() 이렇게 해서 이게 실행될 당시로 판단

```jsx
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

```jsx
const anotherPerson = {
  name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

### 3. 생성자 함수 호출

- 생성자 함수가 미래에 생성할 인스턴스가 바인딩

```jsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

new 랑 같이 안쓰면 생성자 함수 동작이 안됨 → 일반 함수 호출

```jsx
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않는다. 즉, 일반적인 함수의 호출이다.
const circle3 = Circle(15);

// 일반 함수로 호출된 Circle에는 반환문이 없으므로 암묵적으로 undefined를 반환한다.
console.log(circle3); // undefined

// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

→ this 가 윈도우니까 this.radius는 window.radius .

→ radius는 window.radius 그냥 전역 변수 호출처럼 된다.

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

Function.prototype.apply/call/bind

- this 로 사용할 객체를 전달하는 방식.
- apply, call → 즉시 실행하며 this 바인딩, bind → 바로 아니고 호출시에 this 바인딩
- apply(this 바인딩할 객체, 배열나열)  / call (this 바인딩할 객체, 쉼표나열) / bind (this 바인딩객체, 쉼표나열)

원래 일반 함수로 호출 된 경우 전역으로 바인딩인데, 특정 객체에 바인딩 

- 함수 선언

```jsx
function showName() {
  console.log(this.name);
}

const person = { name: 'Lee' };

// 1. 일반 호출
showName();              
// 브라우저: undefined (window.name 기본값 ''일 수도 있음)
// Node.js: undefined

// 2. call → 즉시 실행, this 지정
showName.call(person);   
// "Lee"

// 3. apply → 즉시 실행, this 지정 (인자를 배열로 받음)
showName.apply(person);  
// "Lee"

// 4. bind → this 고정된 새 함수 반환
const boundFn = showName.bind(person);
boundFn();               
// "Lee"
```

- 객체 선언

```jsx
const person = {
  name: 'Lee',
  showName() {
    console.log(this.name);
  }
};

// 1. 메서드 호출
person.showName();       
// "Lee"

// 2. 메서드를 변수에 할당 → 일반 함수 호출됨
const fn = person.showName;
fn();                    
// undefined (this는 전역)

// 3. call
fn.call(person);         
// "Lee"

// 4. apply
fn.apply(person);        
// "Lee"

// 5. bind
const boundFn = fn.bind(person);
boundFn();               
// "Lee"
```

- 콜백 함수

```jsx
const person = {
  name: 'Lee',
  foo(callback) {
    // ①
    setTimeout(callback, 100);
  }
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // ② Hi! my name is .
  // 일반 함수로 호출된 콜백 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
  // 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
  // Node.js 환경에서 this.name은 undefined다.
});
```
