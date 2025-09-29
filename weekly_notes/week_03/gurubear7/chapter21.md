## 빌트인 객체

- 표준, 호스트, 사용자 정의

### 표준 빌트인 객체

애플리케이션 전역의 공통 기능을 제공하는 정의된 객체

별도의 선언 필요없음

```jsx

// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj);     // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj= new Boolean(true); // Boolean {true}
console.log(typeof boolObj);      // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func);                       // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr);        // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp);         // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date();  // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object

```

### 원시값과 래퍼 객체

원시값은 객체가 아닌데, 객체처럼 접근하면 임시 객체가 생성됨 → 래퍼

```jsx
const str = 'hi';
console.log(str.length); // 이렇게 String 객체 인스턴스처럼 가능

// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

### 전역객체

어떤 객체보다도 먼저 생성되는 특수 객체. 브라우저(window, self, this, frames) / 노드 (global)

globalThis로 전역 객체 확인 가능

```jsx
// 브라우저 환경
globalThis === this   // true
globalThis === window // true
globalThis === self   // true
globalThis === frames // true

// Node.js 환경(12.0.0 이상)
globalThis === this   // true
globalThis === global // true
```

→ 전역 변수와 전역 함수를 프로퍼티로 가짐

→ 표준 빌트인 객체를 프로퍼티로 가지고 있음

var 로 선언하거나 선언하지 않은 변수 → 암묵적 전역으로 전역 객체의 프로퍼티 됨

```jsx
// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// 전역 함수
function baz() { return 3; }
console.log(window.baz()); // 3

```

### 빌트인 전역 프로퍼티

전역객체의 프로퍼티

- Infinity - 무한대를 나타내는 숫자값
- NaN - 숫자가 아님을 나타냄
- undefined

### 빌트인 전역 함수

eval - 문자열을 코드로 평가하는 법 → 많은 문제와 보안 취약의 원인이 됨 사용 금지

isFinite - 유한수 인지 검사하는 함수

isNaN - NaN 인지 검사하는 함수

parseInt, parseFloat → 각각 정수, 실수로 변환하는 함수 (값, 진수)

encodeURI / decodeURI

uri를 문자열로 전달받아 이스케이프 처리함.

```jsx
// 완전한 URI
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';

// encodeURI 함수는 완전한 URI를 전달받아 이스케이프 처리를 위해 인코딩한다.
const enc = encodeURI(uri);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
```

→ 반대로 문자열로 해독하는게 decode

encodeURIComponent는 특정 부분만 처리할 수 있음 (쿼리 스트링)
