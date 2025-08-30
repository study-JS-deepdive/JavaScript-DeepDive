### 연산자란

- 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수연산을 수행해 하나의 값을 만든다.

```jsx
// 산술 연산자
5 * 4
// 문자열 연결 연산자
'My name is ' + 'Lee'
// 할당 연산자
color = 'red'
// 비교 연산자 
3 > 5
// 논리 연산자
true && false
// 타입 연산자
typeof 'Hi'
```

# 7.1 산술 연산자

- 피연산자를 대상으로 새로운 값을 만든다
- 연산이 불가능할 경우 NaN을 반환

## 7.1.1 이항 산술 연산자

- 피연산자의 값을 변경하는 효과는 없다
- 새로운 값을 만든다.

![image.png](attachment:53b09bdd-2505-49dd-a2c7-24aada6cf9b5:image.png)

## 7.1.2 단항 산술 연산자

- 1개의 피연산자를 산술 연산하여 숫자 값을 만든다
- 피연산자의 값이 변경된다

```jsx
var x = 1;
x++;
console.log(x) // 2

x--;
console.log(x) // 1
```

- 증가/감소 연산자는 위치의 의미가 있다

```jsx
var x = 5 , result;

// 선할당 후증가
result = x++;
console.log(result, x); //5 6

// 선증가 후할당
result = ++x;
console.log(result, x); //7 7

// 선할당 후감소
result = x--;
console.log(result, x); //7 6

// 선감소 후할당
result = --x;
console.log(result, x); //5 5
```

## 7.1.3 문자열 연결 연산자

- + 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연산자로 동작한다.
- 연산을 위해 피연산자의 값이 강제로 변하기도 한다(암묵적 타입 변환 impolicit operation, 타입 강제 변환 type coercion)

```jsx
// 문자열 연결 연산자
'1' + 2; // -> '12'
'1' + 2; // -> '12'

// 산술 연산자
1 + 2; // -> 3

// true는 1로 타입 변환 된다.
1 + true; // -> 2

// false는 0으로 타입 변환 된다.
1 + false; // -> 1

// null은 0으로 타입 변환된다.
1 + null; // -> 1

//undefined는 숫자로 타입 변환되지 않는다.
+undefined;  // -> NaN
1 + undefined; // -> NaN
```

# 7.2 할당 연산자

- 우항에 있는 피연산자의 결과를 좌항에 변수 에 할당한다

![image.png](attachment:ecd37ca7-c033-4a1e-a3bd-fc9ea1d7acac:image.png)

- 할당문은 표현식인 문이다

```jsx
var x;

// 표현식
console.log( x = 10); // 10
```

- 할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가된다

```jsx
var a,b,c;

// 연쇄 할당. 오른쪽에서 왼쪽으로 진행 
// c = 0 : 0으로 평가된다.
// b = 0 : 0으로 평가된다.
// a = 0 : 0으로 평가된다.

a = b = c = 0;
console.log(a,b,c); // 0 0 0 
```

# 7.2 비교 연산자

- 좌항과 우항의 피 연산자를 비교 후 불리언 값으로 변환( true, false)
- if문 for문의 조건으로 사용

## 7.3.1 동등/일치 비교 연산자

- 좌항과 우항의 값을 같은 값으로 평가하는지 비교해 불리언 반환
- 비교하는 엄격성의 정도가 다름
- 동등 비교 연산자는 느슨한 비교를 한다.
- 일치 비교 연산자는 엄격한 비교를 한다.

![image.png](attachment:f529ef2c-bf6d-46e5-a992-1b88cb69d7bb:image.png)

- 동등 비교 연산자는 암묵적 타입 변환을 통해 타입 일치 시킴

```jsx
5 == 5; //-> true
5 -- '5'; // -> true 
```

- 따라서 결과를 예측하기가 어렵다. 대신 일치 비교 연산자를 사용하자

```jsx
'0' == '';    // -> false
0 == '';      // -> true
0 == '0';     // -> true
```

- 일치 비교 연산자는 좌항과 우항이 타입과 값이 같은 경우에 한해 true를 반환

```jsx
5 === 5;   // -> true

5 === '5'; // -> false

// NaN은 자신과 일치하지 않는 유일한 값
NaN === NaN; // -> false 

Number.isNaN(NaN); // -> true
Number.isNaN(10); // -> false
Number.isNaN(1 + undefined) // -> true
```

<aside>
💡

[Object.is](http://Object.is) 함수를 활용하자

</aside>

```jsx
-0 === +0;          // -> true
Object.is(-0, +0)   // -> false

NaN === NaN;        // -> false
Object.is(NaN, NaN) // -> true
```

- 부동등 비교와 불일치 비교는 동등 비교와 일치 비교의 반대 개념

## 7.3.2 대소 관계 비교 연산자

![image.png](attachment:952cee7b-efe5-449b-898a-72349af079ef:image.png)

- 조건식 ? 조건식이 true일 때 반환 값 : 조건식이 false일 때 반환 값
- 각 상황에 맞게 가독성이 좋은 것을 선택하자

```jsx
// 삼항 연산자
var result = score >= 60 ? 'pass' : 'fail';

// if else 문
var result = if (score >= 60) { result = 'pass'; } else {result = 'fail'; };
```

# 7.5 논리 연산자

- 피연산자를 통해 논리 연산을 실행한다

![image.png](attachment:de4a4d61-a5da-4063-b897-dfb3c5a458f4:image.png)

# 7.6 쉼표 연산자

- 왼쪽 피연산자부터 차례대로 평가하고 마지막 피연산자의 평가가 끝나면 마지막 평가 결과를 반환한다.

```jsx
var x, y, z;
x = 1, y = 2, z =3; // 3
```

# 7.7 그룹 연산자

- 소괄호로 연산자 우선순위를 조절할 수 있다

```jsx
10 * 2 + 3 ; // 23
10 * (2 + 3); // 50
```

# 7.8 typeof 연산자

- 각 피연산자의 데이터 타입을 문자열로 반환한다.
- 반환값이 7개의 데이터타입과 전부 일치하지는 않는다

```jsx
// 숫자 (number)
console.log(typeof 42);           // "number"
console.log(typeof 3.14);         // "number"
console.log(typeof NaN);          // "number" (주의: NaN도 number)

// 문자열 (string)
console.log(typeof "Hello");      // "string"
console.log(typeof `템플릿리터럴`); // "string"

// 불리언 (boolean)
console.log(typeof true);         // "boolean"
console.log(typeof false);        // "boolean"

// undefined
let x;
console.log(typeof x);            // "undefined"

// null (주의!)
console.log(typeof null);         // "object" (JS의 오래된 버그)

// 객체 (object)
let obj = { name: "홍길동" };
console.log(typeof obj);          // "object"

// 배열 (Array도 객체 취급)
let arr = [1, 2, 3];
console.log(typeof arr);          // "object"

// 함수 (function)
function greet() { return "Hi"; }
console.log(typeof greet);        // "function"

// 심볼 (symbol)
let sym = Symbol("id");
console.log(typeof sym);          // "symbol"

// BigInt
let bigNum = 123456789012345678901234567890n;
console.log(typeof bigNum);       // "bigint"
```

- null 타입 확인을 위해서는 일치 연산자를 사용하자

```jsx
var foo = null;

typeof foo === null; // -> false
foo === null;        // -> true
```

# 7.9 지수 연산자

- ES7 에서 도입된 지수 연산자는 좌항을 밑으로, 우항을 지수로 거듭 제곱 연산을 실행한다.

```jsx
// 기본적인 거듭제곱
console.log(2 ** 3);   // 8  (2 * 2 * 2)
console.log(5 ** 2);   // 25 (5 * 5)

// 음수 지수
console.log(2 ** -2);  // 0.25 (1 / (2 ** 2))

// 소수 지수
console.log(9 ** 0.5); // 3  (제곱근)

// 변수 활용
let base = 3;
let exponent = 4;
console.log(base ** exponent); // 81

// 기본적인 거듭제곱
console.log(Math.pow(2, 3));  // 8
console.log(Math.pow(5, 2));  // 25

// 음수 지수
console.log(Math.pow(2, -2)); // 0.25

// 제곱근
console.log(Math.pow(9, 0.5)); // 3

// 변수 활용
let base = 3;
let exponent = 4;
console.log(Math.pow(base, exponent)); // 81
```

# 7.10 그 외 연산자

![image.png](attachment:e6fc9658-26e3-49f7-98ad-224e40fe0d62:image.png)

# 7.11 연산자의 부수 효과

- 대부분의 연산자는 다른 코드에 영향을 주지 않는다
- 할당 연산자, 증가/감소 연산자, dele는 부수 효과가 있다

```jsx
var x;
x = 1;
console.log(x); // 1

x++;
console.log(x); // 2

var o = { a: 1};

delete o.a;
console.log(o); // {}
```

# 7.12 연산자 우선순위

![image.png](attachment:eb2b2501-18bf-4ef7-bdb6-5d34d6a80ee8:image.png)

![image.png](attachment:5ec6ad8b-36af-486d-82ef-84647e33ac3f:image.png)

# 7.13 연산자 결합 순서

![image.png](attachment:bc93d2b2-d767-4224-b129-703bddb6804b:image.png)