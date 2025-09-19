### 22장 this

- **객체**
    - 프로퍼티(상태) 와 메서드(동작)을 하나의 논리적 단위로 묶은 자료구조

- 메서드는 자신이 속한 객체의 프로퍼티(상태)를 참조 및 변경할 수 있어야 함
    - 자신이 속한 객체를 가리키는 식별자를 참조 가능해야함

```jsx
// 객체 리터럴 방식으로 생성 시 식별자 재귀적 참조 가능

const circle = {
  // 프로퍼티
  radius: 5,
  // 메서드 : 상태 데이터를 참조, 조작하는 동작
  getDiameter() {
   
   // 메서드가 자신이 속한 객체의 프로퍼티 혹은 다른 객체 참조 시
   // circle을 참조할 수 있어야 한다.
   return 2 * circle.radius;  
  }
};

console.log(circle.getDiameter()); // 10
```

- 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지 않고 바람직하지 않다.

- **this**
    - 자바스크립트에서 제공하는 식별자
    - 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수
    - 자신이 속한 객체, 인스턴스의 프로퍼티, 메서드 참조 가능
    - **this 가 가리키는 값, this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**

- **바인딩**
    - 식별자와 값을 연결하는 과정
    - this 바인딩 → this 가 가리킬 객체를 바인딩 하는 것

```jsx
const circle = {
	radius: 5,
	getDiameter(){
		
		return 2 * this.radius;
	}
};

console.log(circle.getDiameter()); // 10
```

- 객체 리터럴의 메서드 내부 this → 메서드를 호출한 객체, circle 을 가리킴

```jsx
function Circle(radius){
	// this 는 생성자 함수가 생성할 인스턴스
	return 2 * this.radius;
}

Circle.prototype.getDiameter = function () {
	retrun 2 * this.radius;
}

// 인스턴스 생성
const circle = new Circle(5);

// Circle 생성자 의 프로토타입에 getDiameter 를 정의했으므로 상속 
console.log(circle.getDiameter()); // 10
```

- 자바스크립트의 this는 함수가 호출되는 방식에 따라 this 에 바인딩될 값, this 바인딩이 동적으로 결정된다.

```jsx
console.log(this) // window 

function square(number) {
	
	// 일반 함수 내부 this는 전역객체 window 를 가리킨다
	console.log(this);
	return number * number;
}

const person = {
	
	name : 'Lee',
	getName() {
		// 메서드 내부 this -> 메서드를 호출한 객체
		console.log(this); // name: 'Lee', getName
		return this.name;
	}

};

function Person(name){
	this.name = name;
	// 생성자 함수 내부 this 생성자 함수가 생성할 인스턴스
	console.log(this); 
}

const me = new Person('Lee');
```

---

### 22.2 함수 호출 방식과 this 바인딩

- 함수 호출 방식
    - 일반 함수 호출
    - 메서드 호출
    - 생성자 함수 호출
    - Function.prototype.apply/call/bind 메서드에 의한 간접호출

```jsx
const foo = function () {
	console.dir(this);
};

// 동일 함수도 다양한 방식으로 호출 가능

// 1. 일반 함수 호출
// foo 함수 내부 this -> 전역객체 this
foo() ; // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부 this -> 메서드를 호출한 객체 obj
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부 this -> 생성자 함수가 생성한 인스턴스
new foo(); // foo{}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this 인수에 의해 결정
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```

---

### 22.2.1 일반 함수 호출

- this → 전역객체 바인딩

```jsx
function foo() {
	console.log("foo's this : ", this); // window
	function bar() {
		consol.log("bar's this: ", this); // window
	}
	bar();
}

foo();

// 함수 내부에서 strict 모드 사용 시
// 암묵적 전역변수 금지
// 함수 내부에서만 적용
function foo() {
	'use strict';
	
	console.log("foo's this : ", this); // undefined
	function bar() {
		consol.log("bar's this: ", this); // undefined
	}
	bar();
}
foo();
```

```jsx
// var 키워드로 선언한 전역변수 value -> 전역 객체의 프로퍼티
var value = 1;

const obj = { 
	value: 100,
	foo() {
		console.log("foo's this: ", this); // { value: 100, foo: f }
		console.log("foo's this.value : ", this.value); // 100
		
		// 메서드 내에서 정의한 중첩 함수
		function bar(){
			console.log("bar's this: ", this); // window
			console.log("bar's this.value: " , this.value); // 1
		}
		
		// 메서드 내 정의한 중첩 함수 -> 일반함수로 호출 시 함수 내부 this 
		// 전역 객체가 바인딩
		bar();
	}
};

obj.foo();
```

- 어떤 함수라도 일반 함수로 호출 시 전역 객체가 바인딩된다.
    - 중첩 함수, 콜백함수 내부 this → 전역 객체 바인딩

- 메서드 내부 중첩함수, 콜백 함수의 this 바인딩 → 메서드의 this 바인딩과 일치 시키기 위한 방법

```jsx
var value = 1;

const obj = {
	
	value: 100,
	foo(){
		// this 바인딩(obj)을 변수 that에 할당
		const that = this;
		
		// 콜백 함수 내부 this 대신 that 참조
		setTimeout(function() {
			console.log(that.value); // 100;
		}, 100);
	}
};

obj.foo();
```

---

### 22.2.2 메서드 호출

- 메서드 내부 this → 메서드를 호출한 객체 바인딩

```jsx
const person = {
	
	name: 'Lee',
	getName() {
		// 메서드 내부 this 메서드를 호출한 객체에 바인딩
		return this.name;
	}
};

// 메서드 getName을 호출한 객체 -> person
console.log(person.getName()); // Lee
```

- 메서드는 객체에 포함된 것이 아닌 독립적으로 존재하는 별도의 객체

![image.png](attachment:cceae1a6-6709-4da0-936a-c144a7f44a38:image.png)

---

```jsx
const anotherPerson = {
	name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출 된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 동일
```

```jsx
function Person(name) {
	this.name = name;
}

Person.prototype.getName = function() {
	return this.name;
}

const me = new Person('Lee');

console.log(me.getName()); // Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype
console.log(Person.prototype.getName()); // Kim
```

![image.png](attachment:f80eeef7-7355-4b1a-9fe1-920d86febf49:image.png)

---

### 22.2.3 생성자 함수 호출

- 생성자 함수 내부 this → 생성자 함수가 생성할 인스턴스 바인딩

```jsx
// 생성자 함수
function Circle(radius){
	// 생성자 함수 내부 this -> 생성자 함수가 생성할 인스턴스를 가리킨다.
	this.radius = radius;
	this.getDiameter  = function () {
		return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

---

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call, bind 메서드 → Function.prototype의 메서드

```jsx
function getThisBinding() {
	return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

- apply, call 메서드의 본질적 기능 → 함수를 호출 하는 것

```jsx
function getThisBinding() {
	console.log(arguments);
	return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
// Arguments(3) [1, 2, 3, callee: f, Symbol(Symbol.iterator): f]
// {a: 1}

// call 메서드는 함수의 신수를 리스트 형식으로 전달
console.log(getThisBinding.call(thisArg, 1, 2, 3));
```

- apply, call 메서드의 대표적인 용도 → arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우

- Function.prototype.bind → apply, call 메서드와 달리 함수를 호출하지 않는다.
    - 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성 후 반환한다.

```jsx
function getThisBinding() {
	return this;
}

const thisArg = { a: 1 };

// 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

- bind 메서드 → 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this 가 불일치 하는 문제를 헤결하기 위해 유용하게 사용

---

![image.png](attachment:cd2cab12-3d44-4667-a1bb-452094324883:image.png)
