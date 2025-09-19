### 19장 프로토타입

- 자바스크립트는 명령형, 함수형, **프로토타입 기반 객체지향 프로그래밍**을 지원하는 
멀티 패러타임 프로그래밍 언어
- 프로토타입 기반 객체지향 프로그래밍 언어
- ES6 에서 클래스가 도입
- **원시타입을 제외한, 자바스크립트의 모든 것은 객체**

---

### 19.1 객체지향 프로그래밍

- 프로그램을 객체(Object) 라는 독립 단위로 나누어 설계하고 구현하는 프로그래밍 패러다임
- 실제 세계의 특징을 **속성**으로 프로그래밍에 접목
- 추상화(abstraction)
    - 다양한 속성 중 프로그램에 필요한 속성만 간추려 표현하는 것

 

```jsx
// 이름, 주소 속성을 가진 객체

const person = {
	name : 'Lee',
	adress : 'Korean',
};
```

- **객체**
    - 속성을 통해 여러개의 값을 하나의 단위로 구성한 복합적인 자료구조

```jsx
const circle = {
	 radius: 5, // 반지름 // 프로퍼티
	 
	 // 원 지름 구하기 // 메서드
	 getDiameter(){ 
	  return 2 * this.radius;
	 }
}
```

- 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적 구조
    - 상태 : 프로퍼티
    - 동작 : 메서드

---

### 19.2 상속과 프로토타입

- **상속(inheritance)**
    - 객체지향 프로그래밍의 핵심 개념
    - 객체의 프로퍼티, 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것
    - Js ⇒ 프로토타입 기반 상속 구현
        - 불필요한 중복제거 ⇒ 코드 재사용성 증가 ⇒ 개발 비용 감소

- 프로토타입 기반 상속 구현

```jsx
// 생성자 함수
function Circle(radius){
	this.radius;
}

// Circle 생정자 함수가 생성한 모든 인스턴스가 getArea 메서드를
// 공유하여 사용할 수 있도록 프로토타입에 추가
// 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩 되어있다.

Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
}

// 인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

// 모든 인스턴스는 부모객체 역할인 프로토타입 Circle.prototype 으로부터 getArea 를 상속
// Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유
console.log(circle1.getArea === circle2.getArea); // true
```

![image.png](attachment:1eec6c58-267c-4627-b455-c3e4764eae18:image.png)

- Circle 생성자 함수가 생성한 모든 인스턴스는 Circle.prototype의 
모든 프로퍼티와 메서드를 상속받는다.

---

### 19.3 프로토타입 객체

- 프로토타입 객체(프토토타입)은 객체 간 상속 구현을 위해 사용
- 어떤 객체의 상위 객체 역할을 하는 객체

- 모든 객체는 [[Prototype]] 내부 슬롯을 가짐
    - 객체 생성 방식에 의해 결정됨

- 객체 리터럴에 의해 생성된 프로토타입 ⇒ Object.prototype
- 생성자 함수에 의해 생성된 객체의 프로토타입 ⇒ 생성자 함수의 prototype 프로퍼티에 바인딩된 객체
- 모든 객체는 하나의 프토토타입을 갖는다
- 모든 프로토타입은 생성자 함수와 연결되어 있다.

![image.png](attachment:b16d1390-b4e5-46f0-a14d-7d6611e70368:image.png)

- __proto__ 접근자 프로퍼티를 통해 Prototype 내부슬롯이 카리키는 프로토타입에 간접적 접근 가능

---

### 19.3.1 __proto__ 접근자 프로퍼티

- 모든 객체는 __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입 내부슬롯에 간접 접근 가능

![image.png](attachment:3ff4c76e-761e-4489-b545-74516fe198a3:image.png)

- person 객체의 프로토타입, Object.prototype

- **__proto__ 는 접근자 프로퍼티**
    - value 프로퍼티를 가지지는 않음
    - 접근자 함수 get, set 프로퍼티 어트리뷰트로 구성된 프로퍼티 이다.

```jsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__ 가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;

// setter 함수인 set __proto__ 가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

- **__proto__ 접근자 프로퍼티는 상속을 통해 사용된다.**
    - 객체가 직접 소유하는 프로퍼티가 아닌 Object.prototype의 프로퍼티

- **__proto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**
    - 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해

```jsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cycle __proto__ value
```

- 서로가 자신의 프로토타입이 되는 비정상적인 체인이 만들어지므로 에러를 발생시킨다.
- 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다.

- 코드 내에서 __proto__ 접근자 프로퍼티의 직접 사용은 지양
- 
- 참초 취득 시 Object.getPrototypeOf
- 프로토타입 교체 시 Object.setPrototypeOf 메서드 사용 권장

```jsx
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입 변경
Object.setPrototypeOf(obj. parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

---

### 19.3.2 함수 객체의 prototype 프로퍼티

- 함수 객체만이 소유하는 prototype 프로퍼티 → 생성자 함수가 생성할 인스턴스의 프로토타입
    - 일반객체는 prototype 프로퍼티를 소유하지 않는다.
    - non-constructor 인 화살표함수, 메서드 축약 표현으로 정의한 메서드 → prototype 프로퍼티를 소유하지 않음, 프로퍼티를 생성하지 않음
        - 현재 개발 중 프로젝트 화살표 함수밖에 안썼는데..

- __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 동일한 프로퍼티를 가리키지만 사용 주체가 다르다.

| 구분 | 소유 | 값 | 사용주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 프로토타입 접근, 교체 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체의 프로토타입을 할당하기 위해 사용 |

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Lee');

console.log(Person.prototype === me.__proto__); // true
```

---

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

- 모든 프로토타입은 constructor 프로퍼티를 갖는다.
    - constructor 프로퍼티 → 자신을 참조하고 있는 생성자 함수 를 가리킨다.

```jsx
function Person(name){
	this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person 이다.
console.log(me.constructor === Person); // true
```

---

### 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.

- 리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다.
- 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수는 아니다.
- 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다.
- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

| 리터럴 표기법 | 생성자 함수 | 프로토타입 |
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype |
| 배열 리터럴 | Array | Array.prototype |
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

---

### 19.5 프로토타입의 생성 시점

- 프로토타입은 생성자 함수가 생성되는 시점에 생성된다.
    - 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍 이므로.

```jsx
// 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 생성
console.log(Person.prototype); // { constructor: f }

// 생성자 함수
function Person(name){
  this.name = name;
}

// non-constructor -> 프로토타입이 생성되지 않는다.
const Person = name => {
	this.name = name;
};

// non-constructor -> 프로토타입 미생성
console.log(Person.prototype); // undefined
```

- 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. → 함수 호이스팅

---

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성
- 객체가 생성되기 이전 생성자 함수와 프로토타입은 이미 객체화 되어 존재
    - 이후 생성자 함수 또는 리터럴 표기법으로 객체 생성, 프로토타입 → [[Prototype]] 내부 슬롯에 할당

---

### 19.6 객체 생성 방식과 프로토타입의 결정

- 객체 생성 방법
    - 객체 리터럴
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스(ES6)

- OrdinaryObjectCreate 에 의해 생성된다는 공통점 존재
    - 프로토타입은 추상연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정
    - 인수는 객체가 생성되는 시점에 결정

---

- 객체 리터럴
    - Object.prototype 전달

```jsx
const obj = { x: 1 };

// 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 상속받는다.
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x'));  // true
```

- **Object 생성자 함수에 의해 생성된 객체**
    - 인수 없이 Object 생성자 함수 호출 시 빈 객체 호출
    - Object.prototype 프로토타입 생성
    - 객체 리터럴 방식과 동일한 구조

- **생성자 함수에 의해 생성된 객체의 프로토타입**
    - 생성자 함수의 prototype 프로퍼티에 바인딩된 객체 전달

```jsx
function Person(name) {
	this.name = name;
}

const me = new Person('Lee');
```

- 생성자 함수와 생성자 함수의 prototpye 프로퍼티에 바인딩 된 객체와 생성된 객체 사이 연결 생성

- 프로토타입 Person.prototype에 프로퍼티를 추가, 하위 객체가 상속받을 수 있도록 구현

```jsx
function Person(name){
	 this.name = name;
}

Person.prototype.sayHello = function () {
 console.log(`Hi! My Name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello(); // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim
```

![image.png](attachment:59f11c60-b0df-4cec-865f-7942e7efb871:image.png)

- Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아
자신의 메서드 처럼 사용 가능

---

### 19.7 프로토타입 체인

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.sayHello = function () {
	 console.log(`Hi My Name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProtperty -> Obejct.prototype 의 메서드
console.log(me.hasOwnProperty('name')); // true
```

- Person 생성자 함수에 의해 생성된 me 객체 → Object.prototype의 메서드인 hasOwnProperty를 호출 할 수 있다. me 객체가 Person.prototype 뿐 아니라 Object.prototype 또한 상속 받았다.

- 프로토타입 체인
    - 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘
    - 자바스크립트는 객체 프로퍼티 접근 시 프로퍼티가 없는 경우 
    Prototype 내부슬롯의 참조를 따라 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색

- 프로토타입 체인의 최상위 객체 → Object.prototpye
    - 모든 객체는 Object.prototype 을 상속 받는다.
- 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘

---

### 19.8 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person = (function () {
	
	// 생성자 함수
	function Person(name) {
		this.name = name;
	}

	// 프로토타입 메서드
	Person.prototype.sayHello = function () {
		console.log(`Hi! My name is ${this.name}`);
	}
	
	return Person;
	
}());

const me = new Person('Lee');
me.sayHello = function() {
	console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드 호출, 프로토타입 메서드는 가려진다
me.sayHello();
```

![image.png](attachment:8e6f4a00-9e93-40b9-be9d-4aca475a9ff0:image.png)

- 프로퍼티 섀도잉
    - 상속관계에 의해 프로퍼티가 가려지는 현상
- 오버라이딩
    - 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
- 오버로딩
    - 함수의 이름은 동일하나 매개변수 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식

- 프로토타입 프로퍼티 변경 또는 삭제 시
    - 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다

```jsx
Person.prototype.sayHello = function () {
	console.log(`Hey! My name is ${this.name}`);
};

me.sayHello();

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError
```
