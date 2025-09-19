# 6장 데이터타입

- 데이터 타입 (data type) : 값의 종류
    - 자바스크립트의 모든 값은 데이터 타입을 갖는다

- 총 7개의 데이터 타입 제공
    - 원시타입 / 객체타입

- 원시타입
    - 숫자 타입(number) : 숫자
    - 문자열 타입(string) : 문자열
    - 불리언 타입 (boolean) : 논리적 참(true) 거짓(false)
    - undefined 타입 : var 키워드로 선언된 변수에 암묵적으로 할당
    - null 타입 : 값이 없다는 것을 의도적으로 명시할 때 사용하는 값
    - 심벌 타입
- 객체 타입 : 객체, 함수, 배열 등

- 숫자타입의 값 은 주로 산술연산을 위해 생성, 문자열 타입은 주로 화면에 출력하기 위해 생성

---

### 6.1 숫자 타입

- JS 는 타 언어와 다르게 하나의 숫자 타입만 존재
    - 숫자타입. 64비트 부동소수점 형식

```jsx
// 모두 숫자 타입
var integer = 10;
var doble = 10.12;
var negative = -10;
```

- 정수만을 위한 타입은 없고 모든 수를 실수로 처리한다.
    - 정수로 표기하여도 사실 실수 이다.

```jsx
// 세가지 값 표현 가능
Infinity : 양의 무한대
- Infinity : 음의 무한대
NaN : 산술 연산 불가(not-a-number)

console.log(10 / 0); // Infinity
console.log(10 / -0); // -Infinity
console.log(1 * 'String') // NaN

// 자바스크립트는 대소문자를 구별함
Nan, NAN 등 주의
```

---

### 6.2 문자열 타입

- 텍스트 데이터를 나타내는데 사용
- 작은따옴표, 큰 따옴표 백틱(` `) 으로 텍스트를 감싼다.
    - 가장 일반적인 것은 작은 따옴표를 사용하는 것

```jsx
// 문자열 타입
var string;

string = '문자열'; // 작은따옴표
string = "문자열"; // 큰따옴표
string = `문자열`; // 백틱
```

```jsx
// 따옴표로 감싸지 않은 hello 는 식별자로 인식
var string = hello; 
```

---

### 6.3 템플릿 리터럴

- 멀티라인 문자열, 표현식 삽입, 태그드 템플릿
- ` ` ) 백틱을 사용하여 표현

```jsx
var template = `Template literal`;
console.log(template); // Template literal
```

### 6.3.1 멀티라인 문자열

```jsx
// 이스케이프 시퀀스를 사용하지 않고 줄바꿈 허용, 공백 허용
var template = `<ui>
 <li>home</li>
</ui>`;
```

### 6.3.2  표현식 삽입

```jsx
// 표현식 삽입을 통해 간단히 문자열 삽입 가능
var first = 'Hi';
var last ='happy';

console.log(`반가운 인사는 ${first}, 그러면 기분이 ${last}`;

// 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제로 변환되어 삽입됨
console.log(`1 + 2 = ${1 + 2}`); // 1 + 2 = 3
```

---

### 6.4 불리언 타입

- 참 / 거짓을 나타내는 true 와 false

```jsx
var result = true;
console.log(result); // true

result = false;
console.log(result); // false
```

---

### 6.5 undefined 타입

- 값은 undefined 로 유일
- var 로 선언한 키워드는 암묵적으로 undefined 로 초기화
- 의도적으로 변수에 할당하는것은 권장하지 않음

---

### 6.6 null 타입

- null 타입의 값은 null 이 유일
- 변수에 값이 없다는 것을 의도적으로 명시 할 때 사용

```jsx
var name = 'han'

// name 변수는 han 을 참조하지 않게 됨
han = null;
```

---

### 6.7 심벌 타입

- 변경 불가능한 원시 타입의 값
- 다른 값과 중복되지 않는 유일무이한 값

```jsx
var key = Symbol('key');
console.log(typeof key); // symbol

// 객체 생성
var obj = {};

// 이름이 충돌할 위험이 없는 값 심벌을 프로퍼티 키로 사용
obj[key] = 'value';
console.log(obj[key]); // value

```

---

### 6.8 객체 타입

- 자바스크립트를 이루고 있는 거의 모든 것이 객체

---

### 6.9 데이터 타입의 필요성

- 값을 저장할 때 확보해야 하는 메모리 공간의 크기를 결정하기 위해
- 값을 참조할 때 한번에 읽어들여야 할 메모리 공간의 크기를 결정하기 위해
- 메모리에서 읽어 들인 2진수를 어떻게 해석할지 결정하기 위해

---

### 6.10 동적 타이핑

- C, 자바 → 정적 타입 언어
    - 변수 생성 시 명시적 타입 선언 필요
- JS 는 변수 선언 시 타입을 선언하지 않음
- var, let, const 키워드를 통해 변수 선언
- 어떠한 데이터 타입의 값이라도 자유롭게 할당 가능

- **typeof 연산자로 변수 연산 시 변수에 할당된 값의 데이터 타입을 반환한다**
- 선언이 아닌 할당에 의해 타입이 결정(타입추론) 된며 재할당에 의해 타입이 동적으로 변경된다.

- 동적 타입언어는 유연성은 높지만 신뢰성은 떨어진다.
