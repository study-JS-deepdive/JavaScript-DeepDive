### 21장 빌트인 객체

### 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체
    - ECMAScript 사양에 정의된 객체
    - 전역 공통기능 제공
    - 실행환경과 관계 없이 언제나 사용 가능
    - 전역 객체의 프로퍼티로 제공 → 별도 선언 없이 전역 변수처럼 참조 가능

- 호스트 객체
    - ECMAScript 에는 미정의
    - 자바스크립트 실행 환경에서 추가로 제공하는 객체
    - 브라우저 환경
        - DOM, BOM, fetch…
    - Node.js 환경
        - Node.js 고유 API 호스트 객체로 제공

- 사용자 정의 객체
    - 사용자가 직접 정의한 객체

---

### 21.2 표준 빌트인 객체

- Object, String, Boolean, Math .. 등 약 40여개 객체 제공
- Math, Reflect, JSON 제외 모두 인스턴스 생성 가능한 생성자 함수 객체
    - 프로토타입 메서드, 정적 메서드 제공
    - 생성자 함수가 아닌 빌트인 객체 → 정적 메서드 제공

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj);       // object

// Number 생성자 함수에 의한 객체 생성
const numObj = new Number(123);   // Number { 123 }
console.log(typeof numObj);       // object

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입 => String,prototype
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

- 인스턴스 없이 호출 가능한 정적 메서드 제공

```jsx
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5);  // Number { 1.5 }

// toFixed => Number.prototype 의 프로토타입 메서드
// Number.prototype.toFixed 를 호출하는 것
console.log(numObj.toFixed()); // 반올림 -> 2

// 정적 메서드(static method)
// Number 생성자 함수 자체에 붙어있는 함수
// numObj 에서는 호출 불가
console.log(Number.isInteger(0.5)); // false // 정수 여부 검사 true or false 반환
```

---

### 21.3 원시값과 래퍼 객체

- 래퍼객체
    - 문자열, 숫자, 불리언 값에 대해 객체처럼 접근 시 생성되는 임시 객체
    - 원시값에 객체 접근 방식으로 접근 시 자바스크립트 엔진이 일시적으로 원시값을 연관 객체로 변환
        - 메서드 호출 후 다시 원시값으로 되돌린다.

```jsx
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

- 문자열 래퍼 객체의 인스턴스는 String.prototype 메서드를 상속받아 사용 가능하다.

![image.png](attachment:e8453d0b-ee36-4f88-b468-2d58e384fd24:image.png)

```jsx
const str = 'hello'; 

// str -> 암묵적으로 생성된 래퍼 객체
// str 의 값 -> [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티 동적으로 추가
str.name = 'Lee';

// 식별자 str 은 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 가진다.

// 식별자 str 은 이전에 생성된 래퍼객체와는 다른 객체를 가리킨다.
// 새로운 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined
```

### Q.  [str.name](http://str.name) 시 마다 래퍼객체가 새로 생성되고 제거되는 것인지?

![image.png](attachment:457ec279-a360-4806-a1b5-81cdad9697e1:image.png)

- 문자열, 불리언, 심볼 등은 암묵적으로 생성되는 래퍼 객체에 의해 마치 객체처럼 사용할 수 있다.
- 표준 빌트인 객체의 프로토타입 메서드, 프로퍼티 참조 가능

- String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 인스턴스를 생성하는 것은 권장하지 않는다.

---

### 21.4 전역 객체

- **전역객체**
    - 최상위 객체
    - 코드가 실행되기 이전 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수 객체
    - 브라우저 환경 → window
    - Node.js 환경 → global

- globalThis
    - 브라우저 환경, Node.js 환경에서 전역 객체를 가리키던 식별자를 통일한 식별자

```jsx
// 브라우저 환경
	globalThis === this // ture
	globalThis === window // ture
	

// node 환경
	globalThis === this   // true
	globalThis === global // true
```

- 전역객체의 프로퍼티
    - 표준 빌트인 객체
    - 환경에 따른 호스트 객체
        - 클라이언트 Web API, Node.js → 호스트 API
    - var 키워드로 선언한 전역변수, 전역함수

- 전역객체 가 최상위다 라는 의미
    - 전역 객체는 어떤 객체의 프로퍼티가 아니다.
    - 객체의 계층적 구조 상 빌트인 객체, 호스트 객체를 프로퍼티로 소유한다.

- 전역 객체의 특징
    - 개발자가 의도적으로 생성 불가
    - 전역객체 참조 시 window, global 생략 가능

```jsx
// F -> 16 진수로 해석 10진수로 변환하여 반환
window.parseInt('F', 16); // 15

parseInt('F', 16);        // 15

// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아닌 전역 객체의 프로퍼티
bar = 2; // window.bar = 2
console.log(window.bar); // 2

// let 이나 const 로 선언한 전역변수는 전역 객체의 프로퍼티가 아님
let foo = 123;
console.log(window.foo); // undefined
```

---

### 21.4.1 빌트인 전역 프로퍼티

- 빌트인 전역 프로퍼티
    - 전역 객체의 프로퍼티

- **Infinity**
    - 무한대를 나타내는 숫자값

```jsx
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0); // Infinity

// Infinity 는 숫자값
console.log(typeof Infinity); // number
```

- **NaN ( Not - a - Number )**

```jsx
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```

- **undefined**

```jsx
console.log( window.undefined); // undefined
```

---

### 21.4.2 빌트인 전역 함수

- 애플리 케이션 전역에서 호출 할 수 있는 빌트인 함수

- **eval**
    - 주어진 문자열 코드를 런타임에 평가 또는 실행

```jsx
// 표현식인 문
eval('1 + 2;'); // -> 3
// 표현식이 아닌 문
eval('var x = 5;'); // undefined

// 런타임에 변수 선언문 실행
console.log(x); // 5

// 객체 리터럴, 함수 리터럴은 괄호로 둘러쌓아야 한다.
const o = eval('({ a : 1})');
console.log(o); // {a: 1}

// 전달받은 인수가 여러개의 문일 경우 마지막 결과값 반환
eval('1 + 2; 3 + 4;'); // -> 7

// 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정
const x = 1;

function foo() {

	// eval 함수는 런타임에 foo 함수의 스코프를 동적으로 수정
	eval('var x = 2;');
	console.log(x); // 2

}

foo();
console.log(x); // 1
```

- 함수 호출 시 런타임 이전 , 함수 내부 모든 선언문 실행 후 스코프에 등록
    - eval 함수 호출 시점에는 이미 foo 함수의 스코프 존재

- eval 함수 → 기존 스코프를 동적으로 수정
    - 이미 그 위치에 존재하던 코드처럼 동작
- stirct 모드에서는 기존 스코프 수정 X
- 전달받은 인수가 let, const 키워드를 사용한 변수 선언문인 경우 암묵적으로 strict 모드가 선언됨
    - var 키워드가 잘 사용되지 않으므로 strict 모드가 표준이라고 보면 될듯 하다.

- 보안, 최적화 안좋음
- eval 함수의 사용은 금지해야한다 ;;
    - 다 읽고 금지선언;;

- **isFinite**
    - 전달받은 인수가 유한수 일경우 true, 무한수 이면 false

```jsx
isFinite(0); // true

isFinite(Infinity); // false
```

- **isNaN**
    - 전달받은 인수 NaN 인지 검사
    - 숫자가 아닌 경우 숫자로 타입 변환

```jsx
isNaN(NaN); // true
isNaN(10);  // false

isNaN('10'); // false: '10' => 10
```

- **parseFloat**
    - 전달받은 인수를 실수로 해석하여 변환

```jsx
parseFloat('3.14')'; // 3.14
```

- **parseInt**
    - 전달받은 문자열 인수를 정수로 해석하여 변환

```jsx
parseInt(10);     // 10
parseInt(10.123); // 10
```

- **encodeURI / decodeURI**
    - 완전한 URI 를 문자열로 받아 이스케이프 처리를 위해 인코딩
    - URI : 인터넷에 있는 자원을 나타내는 유일한 주소
- 이스케이프 처리
    - 아스키 문자 셋으로 변환

![image.png](attachment:5e606b93-f18e-49fd-bfad-47f5945e0db3:image.png)

![image.png](attachment:e185f775-c040-4580-b1b5-bac77b48d05f:image.png)

- **encodeURIComponent / decodeURIComponent**
    - URI 구성요소를 인수로 받아 인코딩, 디코딩

---

### 21.4.3 암묵적 전역

```jsx
var x= 10; // 전역 변수

function foo () {
 y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자를 전역에서 참조 가능
console.log(x + y); // 30;
```

- y는 전역 객체의 프로퍼티로 추가된 것 뿐
- y는 변수가 아님, 호이스팅이 발생하지 않음

```jsx
console.log(x) // undefined 로 초기화

console.log(y) // ReferenceError: y is not defined

var x= 10; // 전역 변수

function foo () {
 y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자를 전역에서 참조 가능
console.log(x + y); // 30;

```
