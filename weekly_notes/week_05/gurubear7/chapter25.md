## 클래스

생성자 함수와 프로토타입으로 할 수 있는데, 클래스는 뭐가 다른가?

생성자 함수 + 프로토타입

```jsx
function Circle(radius) {
  this.radius = radius;
  
  }
	Circle.prototype.getDiameter = function() { // 2
	  return 2 * this.radius;
};
```

```jsx
class Circle {
  constructor(radius) {
    this.radius = radius;

  }
  getDiameter() {
    return 2 * this.radius;
  }
}
```

둘은 같은 동작

```jsx
class Circle {
  constructor(radius) {
    this.radius = radius;
  }
  getDiameter() {
    return 2 * this.radius;
  }
}

// constructor 안 -> 인스턴스 프로퍼티
// 밖 -> 프로토타입 메서드

// constructor 안에 메서드를 정의하면, 메서드를 모든 인스턴스가 공유하는게 아니라,
// 인스턴스를 생성할 때 마다 메서드를 새로 생성하는것 (프로토타입은 이미 정의된 거를 공유해서 쓰는 것)
```

### 생성자 함수와 클래스 차이

1. 클래스를 new 연산자 없이 호출하면 에러가 발생한다. 하지만 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출된다.
2. 클래스는 상속을 지원하는 extends와 super 키워드를 제공한다. 하지만 생성자 함수는 extends와 super 키워드를 지원하지 않는다.
3. 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다. 하지만 함수 선언문으로 정의된 생성자 함수는 함수 호이스팅이, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생한다.
4. 클래스 내의 모든 코드는 암묵적으로 strict mode가 지정되어 실행되며 strict mode를 해제할 수 없다. 하지만 생성자 함수는 암묵적으로 strict mode가 지정되지 않는다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false다. 따라서 열거되지 않는다.

### 클래스 구성

constructor(생성자), 프로토타입 메서드, 정적 메서드

```jsx
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

### 인스턴스 생성

- new → constructor를 new랑 함께 호출하여 인스턴스 생성
- new 를 안쓰면 일반함수(생성자 함수에서 [[Call]]), 쓰면 생성자 함수([[Construct]] →  이게 constructor 를 실행)
- 클래스로 정의한 경우 new 안쓰면 에러남(constructor 가 정의되어 있으니까 new를 꼭 써야됨)

### 클래스의 constructor와 프로토타입의 constructor

- 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티, 생성자 함수 가리킴
- 클래스의 constructor 메서드 - 클래스 정의가 평가될 때, constructor에 기술된 동작을 하는 함수 객체 생성

### 클래스에 프로토타입 메서드 추가

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 메서드 축약형으로 명시적으로 추가

```jsx
// me 객체의 프로토타입은 Person.prototype이다.
Object.getPrototypeOf(me) === Person.prototype; // -> true
me instanceof Person; // -> true

// Person.prototype의 프로토타입은 Object.prototype이다.
Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
me instanceof Object; // -> true

// me 객체의 constructor는 Person 클래스다.
me.constructor === Person; // -> true
```

- 클래스가 생성한 인스턴스도 프로토타입의 체인이 된다

### 정적 메서드

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}
```

- 클래스로만 호출할 수 있는 메서드
- 정적 메서드 내부의 this가 인스턴스가 아닌 클래스를 가리킴 vs 프로토타입 메서드는 인스턴스를 가리킴

인스턴스 생성 과정

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 1. new 와 호출하면, 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

### 프라이빗 필드 정의

```jsx
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

프라이빗 → 인스턴스에 공유는 되지만 접근은 안됨

스태틱 → 인스턴스에 공유 자체를 안함

![image.png](attachment:87ed77b7-4c81-43dd-9991-11ed1ff4663c:image.png)

```jsx
class MyMath {
  // static public 필드 정의
  static PI = 22 / 7;

  // static private 필드 정의
  static #num = 10;

  // static 메서드
  static increment() {
    return ++MyMath.#num;
  }
}

console.log(MyMath.PI); // 3.142857142857143
console.log(MyMath.increment()); // 11
```

- 스태틱은 모든 인스턴스가 똑같이 공유해야할 것

### 상속

상위 클래스의 속성(프로퍼티, 메서드)을 하위클래스에서 받아와 쓸 수 있다.

extends를 이용해서 상속 정의

```jsx
class Animal {
  constructor(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  eat() { return 'eat'; }

  move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
  fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird); // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird); // true
console.log(bird instanceof Animal); // true

console.log(bird.eat());  // eat
console.log(bird.move()); // move
console.log(bird.fly());  // fly
```

### super

→ 상위 클래스에 정의된 프로퍼티를 받아서 쓰기

→ 상위 클래스 내용(공통)은 똑같이 쓰고, 인스턴스에서 달라지는 부분만 따로 쓸 때

```jsx
// 수퍼클래스
class Base {
  constructor(a, b) { // ④
    this.a = a;
    this.b = b;
  }
}

// 서브클래스
class Derived extends Base {
  constructor(a, b, c) { // ②
    super(a, b); // ③
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3); // ①
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```

constructor가 없으면 상관없는데(3 무시), constructor로 새로 생성자를 만드는데도 super가 없으면 에러

### 상위 클래스의 메서드 가져오기

```jsx
// 수퍼클래스
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

// 서브클래스
class Derived extends Base {
  sayHi() {
    // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
    return `${super.sayHi()}. how are you doing?`;
  }
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```

순서

```jsx
// 수퍼클래스
class Rectangle {
  constructor(width, height) { --- 2 실행 -> 인스턴스에 반영
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  toString() {
    return `width = ${this.width}, height = ${this.height}`;
  }
}

// 서브클래스  
class ColorRectangle extends Rectangle {
  constructor(width, height, color) { --- 3 실행 -> 인스턴스에 반영
    super(width, height); --- 1 호출
    this.color = color;
  }

  // 메서드 오버라이딩
  toString() {
    return super.toString() + `, color = ${this.color}`;
  }
}

const colorRectangle = new ColorRectangle(2, 4, 'red');
console.log(colorRectangle); // ColorRectangle {width: 2, height: 4, color: "red"}

// 상속을 통해 getArea 메서드를 호출
console.log(colorRectangle.getArea()); // 8
// 오버라이딩된 toString 메서드를 호출
console.log(colorRectangle.toString()); // width = 2, height = 4, color = red
```
