# 15장 let, const 키워드와 블록 레벨 스코프

## 1. var 키워드로 선언한 변수의 문제점

### 1-1. 변수 중복 선언 허용

`var` **키워드로 선언한 변수는 중복 선언이 가능함.**

동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용 발생함.

```jsx
var x = 1; // x변수 선언 & 초기화
var y = 1; // y변수 선언 & 초기화

// var 키워드로 선언한 변수 : 같은 스코프 내에서 중복 선언을 허용
// 초기화문이 있는 변수 선언문은 var 키워드가 없는 것처럼 동작함.
// 초기화문 : 변수 선언과 동시에 초기값을 할당하는 문.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시됨.
var y;

console.log(x); // 100
console.log(y); // 1
```

### 1-2. 함수 레벨 스코프

`var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정함.

따라서 **함수 외부에서** `var` **키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.**

```jsx
var x = 1;

if (true) {
// x는 전역 변수임. 이미 선언된 전역 변수가 있으므로 x는 중복 선언됨.
// 재할당으로 인해 의도치 않게 변수 값이 변경됨.
  var x = 10;
}

console.log(x); // 10
```

함수 레벨 스코프는 전역 변수를 남발할 가능성을 높임 → 의도치 않게 전역 변수가 중복 선언되는 경우가 발생

### 1-3. 변수 호이스팅

`var` 키워드로 변수를 선언하면 변수 호이스팅에 의헤 변수 선언문이 스코프의 선두로 끌어올려진 것처럼 동작함.

```jsx
// 1. 변수 호이스팅에 의해 foo 변수 선언됨
// 2. 변수 foo는 undefined로 초기화됨
console.log(foo); // undefined

// 3. foo 변수에 값 할당
foo = 123;

console.log(foo); // 123

var foo;
```

변수 호이스팅은 에러를 발생시키지 않지만 프로그램의 흐름에 혼란을 주고, 가독성을 떨어뜨리며, 오류를 발생 가능성을 높임.

---

## 2. let 키워드

`var` 키워드의 단점을 보완하기 위해 ES6에서 새로운 키워드 `let`, `const`를 도입함.

### 2-1. 변수 중복 선언 금지

`let` 키워드는 같은 스코프 내에서 변수 중복 선언을 허용하지 않음. ➔  중복 선언하면 문법 에러(SyntaxError)가 발생함.

```jsx
// var : 같은 스코프 내에서 중복 선언 허용
var foo = 123;
var foo = 456;

// let : 같은 스코프 내에서 중복 선언 금지
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 2-2. 블록 레벨 스코프

`let` 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프(block-level scope)를 가짐.

```jsx
// 전역 변수
let foo = 1;

{
// 지역 변수
  let foo = 2;
  let bar = 3;
  console.log(foo); // 2
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined

```

함수도 코드 블록이므로 스코프를 만듦.

![IMG_3638.jpeg](attachment:75cfc07e-7e01-41a8-8efd-435b7e6317c7:IMG_3638.jpeg)

### 2-2. 변수 호이스팅

`let` 키워드로 선언한 변수는 **“선언 단계”와  “초기화 단계”가 분리**되어 진행됨.

➔ 런타임 이전에 선언 단계 실행

 > 변수 선언문에 도달 시 초기화 단계 실행

 > 할당문에서 할당 단계 실행.

```jsx
// var 변수 호이스팅

// 런타임 이전에 선언 및 초기화 단계 실행
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

// 할당문에서 할당 단계 실행
foo = 1;
console.log(foo); // 1

// -------------------------------------

// let 변수 호이스팅과 일시적 사각지대(TDZ)

// 런타임 이전에 선언 단계 실행
// 초기와 이전의 일시적 사각지대에서는 변수 참조 불가
console.log(foo); // Uncaught ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```

변수 선언문 이전에 참조하면 **참조 에러(ReferenceError)**가 발생함. ➔ 이 구간이 일시적 사각지대(TDZ)

일시적 사각지대(TDZ) : 스코프의 시작 지점부터 초기화 시작 지점(변수 선언문)까지 변수를 참조할 수 없는 구간.

![image.png](attachment:1bfda98c-5314-44c4-acac-799c140369e7:image.png)

### 2-3. 전역 객체와 let

`var` 키워드로 선언한 전역 변수는 전역 객체 `window`의 프로퍼티가 됨.

전역 객체의 프로퍼티를 참조할 때 `window`를 생략 가능

- 
    
    ```jsx
    // 이 예제는 브라우저 환경에서 실행해야 함
    
    // 전역 변수
    var x = 1;
    // 암묵적 전역
    y = 2;
    // 전역 함수
    function foo() {}
    
    // var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
    console.log(window.x); // 1
    // 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
    console.log(x); // 1
    
    // 암묵적 전역은 전역 객체 window의 프로퍼티다.
    console.log(window.y); // 2
    console.log(y); // 2
    
    // 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
    console.log(window.foo); // f foo() {}
    // 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
    console.log(foo); // f foo() {}
    ```
    

`let` **키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.**

**즉,** `window` **객체에 접근 할 수 없다.**

- /
    
    ```jsx
    // 이 예제는 브라우저 환경에서 실행해야 함
    
    let x = 1;
    
    // let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
    console.log(window.x); // undefined
    console.log(x); // 1
    
    ```
    

---

## 3. const 키워드

`const` 키워드는 상수 선언 시 사용함. (반드시 상수만을 위해 사용하는 것은 아님)

### 3-1. 선언과 초기화

 - `const` 키워드 변수는 반드시 선언과 동시에 초기화를 해야 한다.

```jsx
const foo = 1; // 변수 선언과 동시에 초기화
const bar;  // SyntaxError: Missing initializer in const declaration
```

 - `let` 키워드처럼 **블록 레벨 스코프**를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작함(**“선언 단계”와  “초기화 단계”가 분리되어 진행됨**).

### 3-2. 재할당 금지

 - `const` 키워드로 선언한 변수는 재할당이 금지됨.

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 3-3. 상수

`const` 키워드로 선언한 변수에 원시 값 을 할당한 경우 변수 값을 변경할 수 없음.

원시 값은 변경 불가능한 값(immutable value) 이므로 재할당 없이 값을 변경할 수 있는 방법이 없기 때문.

**상수(constant) : 재할당이 금지된 변수**

상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용을 권장함

상수 이름 : 일반적으로 대문자로 선언. 여러 단어일 경우 언더스코어(_)로 구분하여 스네이크 케이스로 표현함

```jsx
// 세율을 의미하는 TAX_RATE 라는 상수(constant) 사용
// 변수 이름은 대문자로 선언해 상수임을 명확히 함
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + preTaxPrice * TAX_RATE;

console.log(afterTaxPrice); // 110
```

### 3-4. const 키워드와 객체

`const` 키워드로 선언한 변수에 객체를 할당한 경우는 변수 값을 변경할 수 있음.

```jsx
const person = {
  name: "Lee",
};

// 객체는 변경 가능한 값(mutable value) == 재할당 없이 변경 가능
person.name = "Kim";
console.log(person.name); // { name: "Kim" };
```

---

## 4. var vs. let vs. const

- 변수 선언 시 기본적으로 `const`를 사용하는것이 안전함.
- 변수 재할당이 필요한 경우에만 `let` 사용. 이때, 변수의 스코프는 최대한 좁게 유지.
- `var` 키워드는 사용을 지양.

| 특징 | `var` | `let` | `const` |
| --- | --- | --- | --- |
| 스코프 | 함수 | 블록 | 블록 |
| 재선언 | O | X | X |
| 재할당 | O | O | X |
| 호이스팅 | O (초기화됨) | O (TDZ 있음) | O (TDZ 있음) |
