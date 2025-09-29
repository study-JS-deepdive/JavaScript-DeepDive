## 프로토타입

자바스크립트 멀티 패러다임 - 명령형, 함수형, 프로토타입 기반, 객체지향 프로그래밍

### 객체지향 프로그래밍

프로그램은 독립적 단위인 객체들의 집합이다

- **속성 을 가진 실체들 → 필요한 속성만 간추려 내어 표현 “추상화”**
- 여러개 속성 값을 하나의 단위로 구성한 자료구조 → 객체
- 상태를 나타내는 데이터 + 상태 데이터를 조작하는 동작
- 프로퍼티 + 메서드
- 각 객체는 고유한 기능을 수행하면서 다른객체와 관계성

### 상속과 프로토타입

객체의 프로퍼티 or 메서드를 다른 객체가 상속 받아 씀

- 생성자 함수로 객체를 만들어 쓸 때, 같은 메서드를  계속 중복 생성함
- 프로토타입으로 상속 구현
    
    ```jsx
    function Circle(radius) {
    	this.radius = radius;
    }
    
    Circle.prototype.getArea = function () {
    	return Math.PI * this.radius ** 2;
    };
    
    const circle1 = new Circle(1);
    const circle2 = new Circle(2);
    ```
    
    - 생성자함수(Circle)의 prototype에 메서드 할당
    - 인스턴스들은 이 메서드를 상속받아 사용
    - → 인스턴스들은 프로퍼티만 고유하게. 메서드는 상속으로 공유

### 프로토타입 객체

모든 객체는 [[Prototype]] 이라는 내부 슬롯 가짐

객체 생성 방식에 따라 프로토타입 결정

### ‘__proto__’ 접근자 프로퍼티

객체는 이걸로 자신의 프로토타입 슬롯에 접근 가능

→ 객체가 직접 소유X, 상속으로 Object.prototype.__proto__

### 리터럴 표기법에 의해 생성된 객체의 생성자 함수, 프로토타입

생성자 함수로 만든게 아니라

```jsx
const obj = {};
```

이 때도 가상의 생성자 함수를 갖는 거고, 프로토타입도 더불어 생성됨

### 리터럴로 생성한 경우의 생성자 함수와 프로퍼티

![image.png](attachment:e8feab66-76cd-4924-8a1a-9b62e1dfc2eb:image.png)

### constructor 는 생성자 함수를 가리킨다

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person('Lee');

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person);  // true
```

### 프로토타입 생성 시점

생성자 함수가 생성되는 시점에 더불어 생성

생성자 함수 = 사용자 정의 or 빌트인

화살표 함수는 non-constructor고, 프로토타입 생성 안됨

### 프로토타입 메서드

프로토타입에 프로퍼티를 추가하여 하위 객체가 호출 가능

```jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello();  // Hi! My name is Lee
you.sayHello(); // Hi! My name is Kim

console.log(me.hasOwnProperty('name')); // true
```

### 프로토타입 체인

그보다 더 위의 Object.prototype도 상속 받는다는 것 보여줌

```jsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person('Lee');

// hasOwnProperty는 Object.prototype의 메서드다.
console.log(me.hasOwnProperty('name')); // true
```

→ 접근하려고 할 때, 프로퍼티가 없으면 참조를 따라 부모의 프로토타입도 검색

### 오버라이딩과 프로퍼티 섀도잉

```jsx
const Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHello = function () {
    console.log(`Hi! My name is ${this.name}`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee');

// 인스턴스 메서드
me.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

// 인스턴스 메서드가 호출된다. 프로토타입 메서드는 인스턴스 메서드에 의해 가려진다.
me.sayHello(); // Hey! My name is Lee
```

→ 오버라이딩 상위 클래스의 메서드를 하위에서 재정의

→ 오버로딩 같은 이름의 함수지만 매개변수 타입이나 개수가 다른 메서드 구현

(자바스크립트에서는 안되고, arguments로만)
