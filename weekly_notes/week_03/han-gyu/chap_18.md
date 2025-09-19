### 18장 함수와 일급객체

### 18.1 일급객체

- 일급객체 조건
    - 무명의 리터럴로 생성 가능 → 런타임에 생성 가능
    - 변수, 자료구조 배열에 저장 가능
    - 함수의 매개변수에 전달 가능
    - 함수의 반환값으로 사용 가능

```jsx
// 런타임에 함수 리터럴이 평가, 함수 객체가 생성되고 변수에 할당
// 변수 increase 에 함수 할당
const increase = function (num) {
	return ++num;
};

// 객체에 저장 가능하다.
const auxs = { increase };

// 함수의 매개변수에 전달할 수 있다.
// 함수의 반환값으로 사용할 수 있다.
function makeCounter(auxs) {
	
	let num = 0;
	
	return function () {
	  num = aux(num);
	  return num
	}
}

```

![image.png](attachment:0f4822b0-154a-4072-9d16-0d066864f4c6:image.png)

- 함수는 객체와 동일하게 사용 가능
    - 객체 → 값 이므로 함수는 값과 동일하게 취급

- 함수형 프로그래밍을 가능케 하는 장점
    - 함수의 매개변수로 전달, 반환값으로 사용

### ? 함수형 프로그래밍

![image.png](attachment:3b121cb2-b8b3-4eef-80c5-c6a1a9cf1e1c:image.png)

- 함수를 값으로 다룬다 → 일급 객체로 취급되어 매개변수, 반환값으로 사용 가능
- 순수함수 → 외부 상태변경 없이 동일한 값을 반환
- 가독성 향상, 재사용성 높음, 유지보수성 향상

- 일반 객체와 함수의 차이점
    - 일반 객체 → 호출 불가
    - 함수 객체 → 호출 가능
        - 고유 프로퍼티 소유

---

### 18.2 함수 객체의 프로퍼티

![image.png](attachment:2b728ef5-1ca2-4f7c-a9ab-f88986106b56:image.png)

- Object.getOwnPropertyDescriptors 사용 전체 프로퍼티 확인

![image.png](attachment:d046bd17-dbac-4636-80b2-e8c591499014:image.png)

![image.png](attachment:c9e54e64-6822-4fc0-a642-9cb61d1883d3:image.png)

- length, name, prototype, arguments, caller
    - 함수 객체의 데이터 프로퍼티

- __proto__ 는 함수 객체의 프로퍼티가 아닌 Object.prototype 객체의 프로퍼티를 상속받음
    - 모든 객체가 사용 가능

---

### 18.2.1 arguments 프로퍼티

- argument 객체
    - 함수 호출 시 전달된 인수들의 정보를 담고있는 순회 가능한 유사 배열 객체
    - 함수 내부에서 지역 변수처럼 사용
    - 함수 외부에서는 참조 불가

- 자바스크립트는 매개변수와 인수의 개수가 일치하는 지는 확인하지 않는다.

```jsx
function multiply(x, y) {
	console.log(arguments);
	return x + y;
};

console.log(multiply());        // NaN
console.log(multiply(1, 2, 3)); // 2
```

- 함수 호출 시 함수 몸체 내에서 암묵적으로 매개변수 선언, undefined 로 초기화 후 인수 할당
- 초과된 인수는 무시된다
- 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다

![image.png](attachment:b91084e8-c566-440d-ae12-49607afc5d13:image.png)

- arguments 객체
    - 매개변수를 확정할 수 없는 가변 인자 함수 구현 시 유용

```jsx
function sum() {
  let res = 0;
  
  // length 프로퍼티가 존재하는 유사 배열객체, for 문으로 순회 가능
  for(let i = 0; i < arguments.length; i++) {
	  res += arguments[i];
  }
  
  return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

![image.png](attachment:e4f44647-6e33-43ec-a287-0ff6e4b7c7a3:image.png)

- 유사 배열 객체는 배열이 아니므로 배열메서드 사용 시 에러 발생,

```jsx
// ES6 Rest 파라미터

function sum(... args) {
	 return args.reduce((pre, cur) => pre + cur, 0);
}
// reduce 메서드를 통해 전체 값 더하기
// pre -> 누적값이 담김
// cur -> 현재

console.log(sum(1, 2));
console.log(sum(1, 2, 3, 4, 5,));
```

---

### 18.2.2 caller 프로퍼티 → 생략

- 비표준 프로퍼티
- 사용하지 말자

---

### 18.2.3 length 프로퍼티

- 함수 정의 시 선언한 매개변수의 개수

```jsx
function foo() {};
console.log(foo.length); // 0

function bar(x) {
	 return x;
}
console.log(bar.length); // 1
```

---

### 18.2.4 name 프로퍼티

- 함수 이름
- 함수 객체를 가리키는 식별자를 값으로 갖는다
    - 기본적으로 빈 문자열

```jsx
var nameFunc = function foo() {};
console.log(nameFunc.name); // foo

var anonymousFunc = function() {};
console.log(anonymousFunc.name);

// 함수 선언문 형식
function bar() {}
console.log(bar.name) // bar 
```

---

### 18.2.5 __proto__ 접근자 프로퍼티

- Prototype 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티
- 모든 객체는 Prototype 내부 슬롯을 가짐
    - 상속을 구현하는 프로토 타입 객체를 가리킨다.

```jsx
// 객체 리터럴 방식
const obj = { a : 1 };

// 객체 리터럴로 만든 객체는 내부적으로 new Object()와 동일하게 동작
console.log(obj.__proto__ === Object.prototype); // true

// hasOwnProperty -> Object.prototype 메서드
// 객체 고유의 프로퍼티 인 경우 true 반환
// __proto__ 는 객체 고유 키가 아니므로 false
console.log(obj.hasOwnProperty('__proto__')); // false 

```

---

### 18.2.6 prototype 프로퍼티

- 생성자 함수로 호출할 수 있는 함수객체
- constructor 만 소유하는 프로퍼티
    - 일반객체에는 존재하지 않는다.

```jsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty('prototype'); // true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty('prototype'); // false
```
