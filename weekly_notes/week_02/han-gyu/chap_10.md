### 10장 객체리터럴

### 10.1 객체란 ?

- 자바스크립트 → 객체 기반 프로그래밍 언어
    - 원시 값을 제외한 (함수, 배열, 정규식 등) 모두 객체

- 원시타입
    - 하나의 값만 나타냄
- 객체
    - 다양한 타입의 값을 하나의 단위로 구성한 복합적 자료구조

- **원시값은 변경 불가능, 객체는 변경 가능한 값**

- 객체
    - 0개 이상의 프로퍼티로 구성된 집합

```jsx
var person = {
  
  name : 'Lee',
  age : 20,
 
}

// 2개의 프로퍼티
// 프로퍼티 key, value 로 구성 

// 함수도 프로퍼티 값이 될 수 있다
// 프로퍼티에 할당된 함수는 메서드라 칭한다.
var counter = {
 num: 0,
 increase : function() {
   this.num++;
 } 
};
```

- 프로퍼티 : 객체의 상태를 나타내는 값(data)
- 메서드 : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

- 자바스크립트에서 객체와 함수는 서로 분리해서 생각하기 어려운 개념

---

### 10.2 객체 리터럴에 의한 객체 생성

- 인스턴스
    - 클래스에 의해 생성되어 메모리상에 저장된 객체
    - 클래스는 인스턴스를 생성하기 위한 템플릿

- 자바스크립트의 객체 생성방법
    - 객체 리터럴
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스(ES6)

- 객체 리터럴
    - 가장 일반적인 방법

```jsx
var person = {
   name : 'Lee',
   sayHello : function () {
     console.log(`Hello! My Name is ${this.name}.`);
   } // 메서드
};

// person -> object

// 빈 객체 생성
var empty = {};

console.log(typeof empty); // object
```

- 객체 리터럴의 중괄호는 코드블록이 아니므로 세미콜론이 붙어야 한다
    - 객체 리터럴 : 표현식

---

### 10.3 프로퍼티

- 객체는 프로퍼티의 집합, 프로퍼티는 키(key), 값(value)로 구성

```jsx
var person = {

 name : 'Lee', // key: name, value: 'Lee' 
 age : 28,
 
}

// 프로퍼티 나열 시 , 로 구분
```

- 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.

```jsx
var person = {
 
 firstName: 'HanGyu',
 'last-name': 'Lee', // 식별자 네이밍 규칙을 따르지 않은 경우
  
};
```

```jsx
// 동적 프로퍼티 키 생성

var obj = {};
var key = 'hello';

obj[key] = 'world';
console.log(obj); // {hello: 'world'}

// 빈 문자열 프로퍼티키
// 키에 문자열, 심벌 이외 값 사용 시 암묵적 타입변환 -> 문자열로 변환됨
var space = {

	'' : '',

};

// 숫자 리터럴 사용 시
var numObj = {
  
  0: 1,
  1: 2,
  2: 3,

}

// 내부적으로 0, 1, 2 는 문자열로 변환된다.

// 프로퍼티 중복
var obj = {
  name: 'Lee',
  name: 'Kim',
 };
 
 console.log(obj); // { name : 'Kim' }
 
```

---

### 10.4 메서드

- 자바스크립트 함수 → 일급 객체
- 프로퍼티의 값이 함수인 경우 메서드 라고 칭함

```jsx
var circle = {
  radius: 5,
  
  getDiameter: function () {
   return 2 * this.radius; 
  }
};

console.log(circle.getDiameter()); // 10
```

- **this : 객체 자신을 가리키는 참조변수**

---

### 10.5 프로퍼티 접근

- 마침표 표기법
- 대괄호 표기법

```jsx
var person = {
  
  name: 'Lee',
 
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
// 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키 -> 따옴표로 감싼 문자열
console.log((person['name']); // Lee
```

- 객체에 존재하지 않는 프로퍼티에 접근 시 undefined 반환
- 자바스크립트에서 사용 가능한 유효한 이름이 아니면 대괄호 표기법을 사용해야한다.

---

### 10.6 프로퍼티 값 갱신

```jsx
var person = {
  name: 'Lee',
};

// person 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';

console.log(person); // { name : 'person' }
```

---

### 10.7 프로퍼티 동적 생성

```jsx
var person = {
  name: 'Lee',
};

// age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;

console.log(person);
```

---

### 10.8 프로퍼티 삭제

```jsx
var person = {
  name : 'Lee',
};

delete person.name;

console.log(person); // { } 
```

---

### 10.9 객체 리터럴 확장

- ES6 에서 추가된 객체 리터럴 확장 기능

- 프로퍼티 축약

```jsx
// 기존
var x = 1, y = 2;

var obj = {

 x: x,
 y: y,
 
}

// 프로퍼티 축약 표현
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

- 계산된 프로퍼티 이름

```jsx
const prefix = 'prop';
let i = 0;

const obj = {
 [`${prefix}-${++i}`]: i,
 [`${prefix}-${++i}`]: i,
 [`${prefix}-${++i}`]: i,
};

console.log(obj); // { prop-1: 1, prop-2: 2, prop-3: 3}
```

- 메서드 축약 표현
    - 메서드 축약표현으로 할당한 메서드는 프로퍼티에 할당한 함수와 다르게 동작함

```jsx
const obj = {
 name: 'Lee',
 sayHi(){
   console.log('Hi !' + this.name);
  }
 };
```
