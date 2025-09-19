### 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드로 선언한 변수의 문제점

- 변수의 중복 선언 허용
    
    
    ```jsx
    var x = 1;
    var y = 1;
    
    var x = 100;
    
    // 초기화문이 없는 변수 선언문은 무시된다.
    var y;
    
    console.log(x); // 100
    console.log(y); // 1
    ```
    

### 15.1.2 함수 레벨 스코프

- var 키워드로 선언한 변수 → 함수의 코드 블록만을 지역 스코프로 인정한다.

```jsx
var x = 1;

if(true) {
 var x = 10;
}

console.log(x); // 10
```

### 15.1.3 함수 호이스팅

- 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조가 가능하다.
- 할당문 이전에 변수 참조 시 언제나 undefined 를 반환한다.

```jsx
// 이미 변수는 선언되었음
console.log(foo); // undefined

// 변수에 값을 할당
foo = 123;

console.log(foo);

// 변수선언 -> 런타임 이전에 암묵적 실행
var foo;
```

---

### 15.2 let 키워드

### 15.2.1 변수 중복 선언 금지

- let 키워드로 같은 이름의 변수 중복 선언 시 문법 에러 발생

```jsx
let bar = 123;

let bar = 456; // error
```

### 15.2.2 블록 레벨 스코프

- 모든 코드 블록 ( 함수, if 문, for 문, while 문,  try/catch 문 ) 등을 지역스코프로 
인정하는 블록 레벨 스코프

```jsx
let foo = 1; // 전역 변수

{
  
  let foo = 1; // 지역 변수
  let bar = 2; // 지역 변수
  
}
```

### 15.2.3 함수 호이스팅

```jsx
console.log(foo); // ReferenceError: foo is not defined
let foo;
```

- let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행
- 스코프 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간
    - 일시적 사각지대

```jsx
console.log(foo);

let foo;
console.log(foo);

foo = 1;
console.log(foo);
```

- let, const 를 포함하여 모든 선언은 호이스팅 한다.

### 15.2.4 전역 객체와 let

- var 키워드로 선언한 전역 변수와 전역 함수, 변수에 값을 할당한 암묵적 전역은 전역 객체 window 프로퍼티가 된다.

- let 키워드로 선언한 전역변수는 전역 객체의 프로퍼티가 아님
    - 보이지 않는 개념적인 블록 내에 존재하게 됨

---

### 15.3 const 키워드

- 상수를 선언하기 위하여 사용

### 15.3.1 선언과 초기화

- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.

```jsx
const foo = 1;

const foo; // SyntaxError
```

const 키워드로 선언한 변수는 let 변수와 동일한 레벨의 스코프를 가진다.

- 일반적으로 상수의 이름은 대문자로 나타내며 여러단어로 이루어진 경우 언더스코어로 구분한다

```jsx
const TAX_RATE = 0.1;
```

- const 키워드와 객체
    - const 키워드로 선언된 변수에 객체를 할당할 경우 값 변경이 가능하다.

- const 키워드는 재할당은 금지, 불변을 의미하지는 않는다.
