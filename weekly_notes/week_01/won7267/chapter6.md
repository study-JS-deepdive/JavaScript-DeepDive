**06장 데이터 타입**

**데이터 타입(자료형)** : 값의 종류. 자바스크립트의 모든 값은 데이터 타입을 가짐.

ECMAScript 표준(ECMAScript 2015 (6th Edition), 이하 ES6)은 7개의 데이터 타입을 제공함.

<aside>

원시 타입 (primitive data type)

 : boolean null undefined number string symbol(ES6에서 추가)

객체 타입 (object/reference type)

 : object, array, function … 

</aside>

1. **숫자 타입**

하나의 숫자 타입만 존재함.

정수, 실수 등의 모든 수를 실수로 처리함.

숫자 타입은 추가적으로 3가지 특별한 값들도 표현할 수 있다.

- Infinity : 양의 무한대
- -Infinity : 음의 무한대
- NaN : 산술 연산 불가(not-a-number) ➔ 대소문자 유의할 것.

```jsx
var pInf = 10 / 0;  // 양의 무한대
console.log(pInf);  // Infinity

var nInf = 10 / -0; // 음의 무한대
console.log(nInf);  // -Infinity

var nan = 1 * 'string'; // 산술 연산 불가
console.log(nan);       // NaN
```

1. **문자열 타입**

텍스트 데이터를 나타내는 데 사용.

**작은따옴표('')**, 큰따옴표(""), 백틱(``)으로 텍스트를 감쌈.

1. **템플릿 리터럴**

ES6부터 도입됨. 백틱(``)을 사용해 표현함.

<aside>

 - 멀티라인 문자열

 - 표현식 삽입 : ${}으로 표현식을 감쌈. 이때 표현식의 평가 결과가 문자열이 아니더라도 문자열로 타입이 강제 변환됨.

 - 태그드 템플릿

등의 기능 제공.

</aside>

템플릿 리터럴은 런타임에 일반 문자열로 변환되어 처리됨.

1. **불리언 타입**

논리적 참, 거짓을 나타내는 true와 false 뿐임.

```jsx
var foo = true;
var bar = false;

// typeof 연산자는 타입을 나타내는 문자열을 반환한다.
console.log(typeof foo); // boolean
console.log(typeof bar); // boolean
```

1. **undefined 타입**

**undefined** 타입의 값은 undefined가 유일함.

**var 키워드로 선언 이후 값을 할당하지 않은 변수는 undefined로 초기화**됨. 즉, 선언은 되었지만 값을 할당하지 않은 변수에 접근하거나 참조하면 undefined가 반환된다.

```jsx
var foo;
console.log(foo); // undefined
```

 

* 변수의 값이 없다는 것을 명시하고 싶은 경우 ➔ undefined를 할당하는 것이 아니라 null을 할당한다.

1. **null 타입**

null 타입의 값은 null이 유일함.

프로그래밍 언어에서 null은 변수에 값이 없다는 것을 의도적으로 명시할 때 사용함.

```jsx
var foo = 'Lee';
foo = null;  // 이전 참조 정보를 제거.
```

또는 함수가 호출되었으나 유효한 값을 반환할 수 없는 경우, 명시적으로 null을 반환하기도 한다. 예를 들어, 조건에 부합하는 HTML 요소를 검색해 반환하는 Document.querySelector()는 조건에 부합하는 HTML 요소를 검색할 수 없는 경우, null을 반환한다.

```jsx
var element = document.querySelector('.myElem');
// HTML 문서에 myElem 클래스를 갖는 요소가 없다면 null을 반환한다.
console.log(element); // null
```

자바스크립트 오류로 인해 null 타입을 확인할 때 typeof 연산자를 사용하면 안되고 일치 연산자(===)를 사용하여야 함.

```jsx
var foo = null;
console.log(typeof foo === null); // false
console.log(foo === null);        // true
```

1. **심벌 타입**

1. **객체 타입**

자바스크립트를 이루고 있는 거의 “모든 것”이 객체.

6가지 원시 타입(Primitives)을 제외한 나머지 값들(배열, 함수, 정규표현식 등)은 모두 객체임.

1. **데이터 타입의 필요성**

데이터 타입에 의한 효율적인 메모리 공간의 확보와 참조.

데이터 타입에 의한 값의 해석.

1. **동적 타이핑**

**동적 타입 언어와 정적 타입 언어**

 - 정적 타입 언어 : C, C++, 자바 등. 변수를 선언할 때 타입을 선언함.(명시적 타입 선언)

 - 동적 타입 언어 : 자바스크립트, 파이썬, PHP, 루비 등

자바스크립트는 정적 타입 언어와 다르게 변수를 선언할 때 타입을 선언하지 않음.

따라서 어떠한 타입의 값이라도 자유롭게 할당할 수 있음.

변수의 데이터 타입 조사 시 typeof 연산자(뒤에 오는 피연산자의 타입을 문자열로 반환함) 를 사용할 것.

**자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(타입 추론)됨. 그리고 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있음. ➔ 동적 타이핑**

**동적 타입 언어와 변수**

동적 타입 언어는 값에 타입을 자유롭게 할당할 수 있어 편리하지만 단점도 있음.

값이 언제든지 변경될 수 있기 때문에 변화하는 변수 값을 추적하기 어렵고 값을 확인하기 전에는 타입을 확신할 수 없음.

또한 자바스크립트 엔진에 의해 개발자의 의도와 다르게 타입이 변환되기도 해 오류를 유발할 수 있음.

**➔ 동적 타입 언어는 유연성은 높지만 신뢰성은 떨어짐**

따라서 몇 가지 주의사항이 있음.(73p 참고)
