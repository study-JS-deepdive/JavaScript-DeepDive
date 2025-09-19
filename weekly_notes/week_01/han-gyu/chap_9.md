### 9장 타입 변환과 단축평가


### 9.1 타입 변환이란?

- 명시적 타입변환 / 타입 캐스팅
    - 개발자가 의도적으로 값의 타입을 변환 하는 것

```jsx
var x = 10;

// 명시적 타입 변환
var str = x.toString();
```

- 암묵적 타입 변환 / 타입 강제 변환

```jsx
var x = 10;

var str = x + '';
```

---

### 9.2 암묵적 타입 변환

- 개발자의 의도와 상관없이 코드 문맥을 고려, 자바스크립트 엔진의 타입 강제 변환

```jsx
'10' + 2 -> '102'

// 피연산자가 모두 숫자 타입 이어야 하는 문맥
5 * '10' -> 50

// 피연산자 또는 표현식이 불리언 타입이어야 하는 문맥
!0 // true
if(i) {}
```

---

### 9.3 명시적 타입 변환

- 문자열로 변환
    - String 생성자 함수 호출
    - toString 메서드 사용
    - 문자열 연결 연산자 이용

```jsx
// 문자열 타입으로 변환
String(1); // -> '1'
String(true); // -> 'true'

(1).toString(); // -> '1'
(NaN).toString(); 

1 + ''; -> '1';

```

- 숫자 타입으로 변환
    - Number 생성자 함수 호출
    - parseInt, parseFloat 함수 사용
    - + 단항 산술 연산자 이용
    - * 산술 연산자 이용

```jsx
Number('0'); // 0

parseInt('0');

+ '0'; // -> 0

'0' * 1 // -> 
```

- 불리언 타입으로 변환
    - Boolean 생성자 함수 호출
    - !! 부정 논리 연산자 두번 사용

```jsx
Boolean('x');
Boolean(0);

!!'x'; -> true
!!''; -> false
```

---

### 9.4 단축 평가

- 논리 연산자를 사용한 단축 평가

```jsx
'Cat' && 'Dog' -> 'Dog' // 두 피연산자가 모두  true 로 평가될  때 true
'Cat' || 'Dog' -> 'Cat' // 하나만이라도 true 여도 true 로 평가
```

- 옵셔널 체이닝

```jsx
// 좌항의 피연산자가 null, undefined 일 경우 undefined 반환, 그렇지 않으면 우항의 프로퍼티 참조
var elem = null;

var value = elem?.value;
console.log(value);
```

- null 병합 연산자
    - 좌항의 연산자가 null 또는 undefined 일 경우 우항의 피연산자 반환, 그렇지 않으면 좌항의 피연산자 반환

```jsx
var foo = null ?? 'default string';
console.log(foo); // 'default string'
```
