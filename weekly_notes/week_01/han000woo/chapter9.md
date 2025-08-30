# 9.1 타입 변환이란?

- 자바스크립트의 모든 값은 타입이 있다.
- 개발자가 의도적으로 값의 타입을 변경하는것을 명시적 타입 변환 또는 타입 캐스팅이라 한다.

```jsx
var x = 10;

var str = x.toString();
console.log(typeof str, str); // string 10

// 피연산자의 값이 변견된 것은 아님
console.log(typeof x, x); //number 10
```

- 개발자의 의도와 상관없이 자바스크립트 엔진이 타입을 변환하기도 한다
이를 암묵적 타입 변환, 타입 강제 변환이라고 한다.

```jsx
var x = 10;

var str = x +'';
console.log(typeof str, str); // string 10

// 피연산자의 값이 변견된 것은 아님
console.log(typeof x, x); //number 10
```

![image.png](attachment:fb89f996-2ba7-456f-9eb9-17c26737e3b7:image.png)

# 9.2 암묵적 타입 변환

- 자바스크립트 엔진은 코드의 문맥을 고려해 암묵적으로 타입을 변환할 때가 있다.

## 9.2.1 문자열 타입으로 변환

- + 연산자는 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다.

```jsx
// 문자열 연산자 '+'와 다른 타입 간 암묵적 타입 변환 예제

console.log("문자열 + 숫자:");
console.log("5" + 2); // "52" → 숫자 2가 문자열로 변환
console.log(2 + "5"); // "25" → 숫자 2가 문자열로 변환

console.log("\n문자열 + boolean:");
console.log("Value: " + true);  // "Value: true" → boolean true가 문자열로 변환
console.log("Value: " + false); // "Value: false" → boolean false가 문자열로 변환

console.log("\n문자열 + null / undefined:");
console.log("Null: " + null);       // "Null: null" → null이 문자열로 변환
console.log("Undefined: " + undefined); // "Undefined: undefined" → undefined가 문자열로 변환

console.log("\n문자열 + Symbol:");
try {
    console.log("Symbol: " + Symbol("id")); // TypeError! Symbol은 문자열로 암묵 변환 불가
} catch (e) {
    console.log("Symbol: TypeError 발생 (" + e.message + ")");
}

console.log("\n여러 타입 혼합:");
console.log(1 + 2 + "3"); // "33" → 1+2=3 (숫자), "3"+3 → "33"
console.log("3" + 1 + 2); // "312" → "3"+1 → "31", "31"+2 → "312"
console.log("True + null: " + (true + null)); // "True + 1" ? 실제로는 1 + 0 = 1, 그 후 문자열로 변환 → "True + 1"

// 숫자가 아닌 연산자는 숫자로 변환되지 않고 문자열로 결합됨
console.log("false + 'abc': " + (false + 'abc')); // "falseabc"
```

## 9.2.2 숫자 타입으로 변환

- 산술 연산자 표현식 평가를 위해 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 변환한다.
- 변환할 수 없는 경우 평과 결과는 NaN이 된다.

```jsx
1 - '1'    // -> 0
1 * '10'   // -> 10
1 / 'one'  // -> NaN
```

- 비교 연산자도 숫자 타입으로 변환한다

```jsx
'1' > 0 // -> true 
```

- + 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입으로 암묵적 변화를 수행한다

```jsx
// 문자열 타입
+''                   // -> 0
+'0'                  // -> 0
+'1'                  // -> 1
+'string'             // -> NaN

// 불리언 타입 
+true                 // -> 1
+false                // -> 0

// null 타입
+null                 // -> 0

// undefined 타입
+undefined            // -> NaN

// Symbol 타입
+Symbol()             // -> TypeError: Cannot convert a Symbol value to a number

// 객체 타입 
+{}                   // -> NaN
+[]                   // -> 0
+[10, 20]             // -> NaN
+(function(){})       // -> NaN
```

## 9.2.3 불리언 타입으로 변환

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 참, 거짓으로 구분한다

```jsx
// 불리언 타입으로 암묵적 변환 예제

console.log("=== Falsy 값 ===");
console.log(Boolean(false));      // false
console.log(Boolean(0));          // false
console.log(Boolean(-0));         // false
console.log(Boolean(""));         // false
console.log(Boolean(null));       // false
console.log(Boolean(undefined));  // false
console.log(Boolean(NaN));        // false

console.log("\n=== Truthy 값 ===");
console.log(Boolean(true));       // true
console.log(Boolean(1));          // true
console.log(Boolean(-1));         // true
console.log(Boolean("0"));        // true → 빈 문자열이 아닌 문자열
console.log(Boolean("false"));    // true → 문자열 "false"도 truthy
console.log(Boolean([]));         // true → 빈 배열도 truthy
console.log(Boolean({}));         // true → 빈 객체도 truthy
console.log(Boolean(Symbol()));   // true → Symbol도 truthy

console.log("\n=== 조건문에서 암묵적 변환 ===");
if ("hello") {
    console.log("'hello'는 true로 평가됨"); // 실행됨
}
if (0) {
    console.log("0은 true로 평가됨");       // 실행되지 않음
}
if (null) {
    console.log("null은 true로 평가됨");    // 실행되지 않음
}
```

# 9.3 명시적 타입 변환

## 9.3.1 문자열 타입으로 변환

- String 생성자 함수를 new 연산자 없이 호출
- Object.prototype.toString 메서드를 사용
- 문자열 연결 연산자 이용

```jsx
// 문자열 타입으로 명시적 변환 예제

console.log("=== 1. String 생성자 함수 호출 ===");
console.log(String(123));        // "123"
console.log(String(true));       // "true"
console.log(String(null));       // "null"
console.log(String(undefined));  // "undefined"
console.log(String(Symbol("id"))); // "Symbol(id)"

console.log("\n=== 2. toString() 메서드 사용 ===");
console.log((123).toString());       // "123" → 숫자 123을 문자열로 변환
console.log(true.toString());        // "true"
console.log([1,2,3].toString());     // "1,2,3" → 배열도 문자열로 변환
console.log({a:1}.toString());       // "[object Object]" → 객체는 기본 문자열 반환
// console.log(null.toString());     // TypeError 발생 → null은 toString() 없음
// console.log(undefined.toString());// TypeError 발생 → undefined도 없음
// console.log(Symbol("id").toString()); // "Symbol(id)" → Symbol은 toString() 있음

console.log("\n=== 3. 문자열 연결 연산자 '+' 이용 ===");
console.log(123 + "");          // "123"
console.log(true + "");         // "true"
console.log(null + "");         // "null"
console.log(undefined + "");    // "undefined"
// console.log(Symbol("id") + ""); // TypeError
```

## 9.3.2 숫자 타입으로 변환

- Number 생성자 함수를 new 연산자 없이 호출
- parseInt, parseFloat 함수를 사용
- + 단항 산술 연산자 이용
- * 산술 연산자 이용

```jsx
// 숫자 타입으로 명시적 변환 예제

console.log("=== 1. Number 생성자 함수 호출 ===");
console.log(Number("123"));       // 123 → 문자열 숫자를 숫자로 변환
console.log(Number("123.45"));    // 123.45
console.log(Number(true));        // 1
console.log(Number(false));       // 0
console.log(Number(null));        // 0
console.log(Number(undefined));   // NaN
console.log(Number("abc"));       // NaN → 숫자로 변환 불가

console.log("\n=== 2. parseInt / parseFloat 함수 사용 ===");
console.log(parseInt("123"));        // 123
console.log(parseInt("123.45"));     // 123 → 소수점 이하 제거
console.log(parseFloat("123.45"));   // 123.45
console.log(parseInt("10px"));       // 10 → 문자열 앞부분 숫자만 추출
console.log(parseFloat("10.5px"));   // 10.5
console.log(parseInt("abc123"));     // NaN → 숫자 시작이 아니면 NaN

console.log("\n=== 3. + 단항 산술 연산자 사용 ===");
console.log(+"123");       // 123
console.log(+"123.45");    // 123.45
console.log(+true);        // 1
console.log(+false);       // 0
console.log(+null);        // 0
console.log(+undefined);   // NaN

console.log("\n=== 4. * 산술 연산자 사용 ===");
console.log("123" * 1);    // 123 → 문자열 "123"을 숫자로 변환
console.log("123.45" * 1); // 123.45
console.log(true * 1);     // 1
console.log(false * 1);    // 0
console.log(null * 1);     // 0
console.log(undefined * 1);// NaN
console.log("abc" * 1);    // NaN → 숫자로 변환 불가
```

## 9.3.3 불리언 타입으로 변환

- Boolean 생성자 함수를 new 연산자 없이 호출
- ! 부정 논리 연산자를 두 번 사용

```jsx
// 불리언 타입으로 명시적 변환 예제

console.log("=== 1. Boolean 생성자 함수 호출 ===");
console.log(Boolean(1));         // true
console.log(Boolean(0));         // false
console.log(Boolean("hello"));   // true
console.log(Boolean(""));        // false
console.log(Boolean(null));      // false
console.log(Boolean(undefined)); // false
console.log(Boolean([]));        // true → 빈 배열도 truthy
console.log(Boolean({}));        // true → 빈 객체도 truthy

console.log("\n=== 2. !! (부정 논리 연산자 두 번 사용) ===");
console.log(!!1);         // true
console.log(!!0);         // false
console.log(!!"hello");   // true
console.log(!!"");        // false
console.log(!!null);      // false
console.log(!!undefined); // false
console.log(!![]);        // true
console.log(!!{});        // true
```

# 9.4 단축 평가

<aside>
💡

단축 평가는 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것

</aside>

## 9.4.1 논리 연산자를 사용한 단축 평가

![image.png](attachment:06374e22-56cb-4e31-b6d2-1851185ab3fd:image.png)

- 논리곱 연산자 표현식으로 if문을 대체할 수 있다
- 조건이 true일 경우

```jsx
var done = true;
var message = '';

// 주어진 조건이 true일 때 
if (done) message = '완료';

// if문은 단축 평가로 대체 가능하다.
// done이 true라면 message에 '완료'를 할당한다
message = done && '완료';
console.log(message); 
```

- 조건이 false일 경우

```jsx
var done = false;
var message = '';

// 주어진 조건이 false일 때 
if (!done) message = '미완료';

// if문은 단축 평가로 대체 가능하다.
// done이 false라면 message에 '미완료'를 할당한다
message = done || '완료';
console.log(message); 
```

<aside>
💡

단축 평가가 유용하게 사용되는 상황들 

</aside>

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined 인지 확인하고 프로퍼티를 참조할 때

```jsx
//객체란 키와 값으로 구성된 프로퍼티의 집합 
var elem = null;
var value = elem.value; // TypeError: cannot read property

//이때 단축평가를 사용하면 에러 발생하지 않음 
var value = elem && elem.value; 
elem을 확인후 null이면 null을 리턴
null이 아닐 경우 elem.value를 평가 후 리턴 
```

- 함수 매개변수에 기본값을 설정할 때

```jsx
function getStringLength(str) {
  str = str || ''; // str이 null일경우 ''을 반환
  return str.length;
}

getStringLength();
getStringLength('hi');

function getStringLength(str = '') { // str이 null일경우 ''을 반환
  return str.length;
}

getStringLength();
getStringLength("hi");
```

## 9.4.2 옵셔널 체이닝 연산자

- ES11에서 도입됨
- 좌항이 null또는 undefined일 경우 undefined 반환 아니면 우항 반환

```jsx
var elem = null
var value = elem?.value;
console.log(value); // undefined 
```

- 옵셔널 체이닝 도입 이전에는 &&를 사용한 단축평가를 통해 null 여부를 확인했다

```jsx
var elem = null
var value = elem && elem.value;
console.log(value); //null
```

- 논리 연산자 &&는 좌항이 false값(false, undefined, null, 0, -0, NaN, ‘’)이면 그대로 반환한다.

```jsx
var str = '';
// 문자열 길이 참조
var length = str && str.length;

console.log(length) // ''
```

- 옵셔널 체이닝 연산자 ?.는 false로 평가되는 false값이라도 null과 undefined가 아니면 우항의 참조를 이어간다

```jsx
var str ='';
var length = str?.length;
console.log(length); // 0
```

## 9.4.3 null 병합 연산자

- ES11에서 도입됨
- 좌항이 null또는 undefined일 경우 우항 반환, 그렇지 않으면 좌항 반환
- 기본값을 설정할 때 유용하다

```jsx
var foo = null ?? 'default string';
console.log(foo); // "default string" 
```

- null 병합 연산자 도입 이전에는 논리 연산자 ||를 사용한 단축 평가를 통해 기본값을 설정
- 피연산자가 false값이면(flase, undefined, null, 0, -0, NaN, ‘’)이면 우항 반환

```jsx
var foo = '' || 'default string';
console.log(foo); // default string
```

- 하지만 null 병합 연산자 ??는 좌항의 피연산자가 false로 평가되는 값이라도 null또는 undefined가 아니면 좌항 반환

```jsx
var foo = '' ?? 'default string';
console.log(foo); // ""
```