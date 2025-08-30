<aside>
💡 자바스크립트(ES6)는 7개의 데이터 타입을 제공한다.

</aside>

![image.png](attachment:42bf02cd-57b7-442d-bd8b-07a47ad53b57:image.png)

# 6.1 숫자 타입

- C나 JAVA의 경우 정수, 실수 등과 같은 다양한 숫자 타입을 제공한다.
- JavaScript는 숫자 타입 하나만을 제공한다(64비트 부동소수점)
- 2,8,16 진수 모두 메모리에는 64비트 부동소수점 형식 2진수로 저장된다.

```jsx
var bianry = 0b1000001;
var octal = 0o101; 
var hex = 0x41;

console.log(binary);   //65
console.log(octal);    //65
console.log(hex);      //65

console.log(binary === octal); // true
console.log(binary === hex);   // true
```

- 모든 수를 실수로 처리하기에 정수로 표시한다 해도 사실은 실수이다

```jsx
console.log( 1 === 1.0 ) // true 
```

- Infinity, -Infinity, NaN 3 가지 특별한 값도 표현할 수 있다.

# 6.2 문자열 타입

- 큰따옴표 “” , 작은따옴표 ‘’ , 백틱 `` 으로 감싸면 텍스트로 표현할 수 있다.
- C는 문자열 타입을 제공하지 않고 문자의 배열로 문자열을 표현한다.
- Java는 문자열을 객체로 표현한다.
- JavaScript의 문자열은 원시 타입이며 변경 불가능한 값이다.

```jsx
var string = "Hello world ";
console.log(string[0]);
string[0] = "new"
console.log(string);
--------------------------------------------------------------
H
Hello world 
```

# 6.3 템플릿 리터럴

- ES6부터 템플릿 리터럴이라고 하는 새 문자열 표기법 도입

### 템플릿 리터럴

- 백틱으로 문자열 표현

```jsx
var template = `Template literal`;
console.log(template);                  // Template literal
```

## 6.3.1 멀티라인 문자열

- 일반 문자열 내에서는 줄바꿈이 허용되지 않는다

```jsx
var str = 'Hello
world.';
------------------------------------------------
SyntaxError: Invalid or unexpected token
```

- 이를 표현하기 위해 \를 확용한 이스케이프 시퀸스 사용

![image.png](attachment:c0a02377-fcb6-419e-baa5-3aeb39a2c336:image.png)

```jsx
// 줄바꿈과 탭
let text1 = "Hello\nWorld";
console.log(text1);

let text2 = "이름:\t철수";
console.log(text2);

// 따옴표 출력
let text3 = "He said, \"Javascript is fun!\"";
console.log(text3);

// 역슬래시 출력
let text4 = "경로: C:\\Users\\Admin";
console.log(text4);

// 유니코드(예: A = U+0041, 한글 '가' = U+AC00)
let text5 = "\u0041\uAC00";
console.log(text5);
-----------------------------------------------------------------------------------------
$ node index.js 
Hello
World
이름:   철수
He said, "Javascript is fun!"
경로: C:\Users\Admin
A가
```

## 6.3.2 표현식 삽입

- 문자열은 문자열 연산자 +를 사용해 연결할 수 있다

```jsx
//일반 문자열 사용 및 문자열 연산자
let title = "자바스크립트";
let desc = "문자열 비교 예제";

// 일반 문자열: 줄바꿈 불가 → \n과 + 연산 필요
let htmlNormal = 
  "<div>\n" +
  "  <h1>" + title + "</h1>\n" +
  "  <p>" + desc + "</p>\n" +
  "</div>";

console.log(htmlNormal);

//백틱 문자열 사용 및 표현식 삽입  
let htmlTemplate = `
<div>
  <h1>${title}</h1>
  <p>${desc}</p>
</div>
`;

console.log(htmlTemplate);
-------------------------------------------------------------------------
$ node index.js 
<div>
  <h1>자바스크립트</h1>
  <p>문자열 비교 예제</p>
</div>

<div>
  <h1>자바스크립트</h1>
  <p>문자열 비교 예제</p>
</div>
```

- 표현식 삽입은 템플릿 리터럴 내에서 사용해야 한다

```jsx
console.log(`1 + 2 = ${1 + 2}`); // 1 + 2 = 3 
console.log('1 + 2 = ${1 + 2}'); // 1 + 2 = ${1 + 2} 
```

# 6.4 불리언 타입

- 논리적 참, 거짓을 나타내는 true와 false 뿐이다

# 6.5 undefined 타입

- undefined는 javascript 엔진이 변수를 초기화 할때 사용하는 값이다.
- 아무것도 없음을 나타낼땐 null을 사용

![image.png](attachment:ee0b272f-6998-445c-a7b8-9941003c13d9:image.png)

[https://inpa.tistory.com/entry/📚-null-undefined-NaN](https://inpa.tistory.com/entry/%F0%9F%93%9A-null-undefined-NaN)

# 6.6 null 타입

- null 타입의 값은 null이 유일하다
- 의도적으로 값이 없다는것을 나타낼때 사용한다. 곧 가비지 컬렉터가 없앨 것이다.
- 함수가 유효한 값을 반환할 수 없을때 명시적으로 null을 반환하기도 함(document.querySelector)

# 6.7 심벌 타입

- ES6에서 추가된 7번째 타입, 변경 불가능한 원시 타입의 값
- 다른 값과 중복되지 않은 유일무이한 값 → 객체의 유일한 속성키를 만들기 위해 사용

```jsx
// Symbol 생성
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2); // false, 설명: 같은 설명을 넣어도 Symbol은 항상 고유함

// 객체의 고유 키로 Symbol 사용
const user = {
    name: "Alice",
    age: 25,
    [id1]: 1234, // Symbol을 키로 사용
    [id2]: 5678
};

console.log(user.name); // "Alice"
console.log(user[id1]); // 1234
console.log(user[id2]); // 5678

// 일반적인 키와는 달리 for...in이나 Object.keys에서는 Symbol 키가 나오지 않음
for (let key in user) {
    console.log(key); // name, age
}

console.log(Object.keys(user)); // ["name", "age"]
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(id), Symbol(id)]

```

# 6.8 객체 타입

- 자바스크립트는 객체 기반 언어
- 자바스크립트를 이루고 있는 거의 모든 것이 객체이다

# 6.9 데이터 타입의 필요성

## 6.9.1 데이터 타입에 의한 메모리 공간의 확보와 참조

- 값은 메모리에 저장하고 참조할 수 있어야 한다.
- 저장할 때 변수에게 타입만큼의 메모리 공간을 부여한다.
- 참조할때는 해당 변수의 메모리 주소로부터 타입만큼의 공간을 참조한다.

![image.png](attachment:37bdf5a1-4949-412d-bf4d-6ce0e225dfbf:image.png)

## 6.9.2 데이터 타입에 의한 값의 해석

- 메모리에 있는 모든 값은 2진수로 저장된다.
- 값을 읽고 해석하기 위해서는 해당 타입을 알아야 한다.

1. **값을 저장할 때** 확보할 **메모리 공간의 크기** 결정 
2. **값을 참조할 때** 읽어 들여야 할 **메모리 공간의 크기** 결정 
3. 메모리에서 읽어 들인 **2진수를 어떻게 해석**할지 결정 

# 6.10 동적 타이핑

- 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정(Type Inference 타입 추론)된다.
- 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다. - **동적 타이핑**이라 한다.
- 변수는 타입을 가지지 않지만 값은 타입을 갖는다.
- 타입을 결정하는 주체는 값이다.

## 6.10.2 동적 타입 언어와 변수

- 변수 값은 언제든지 변경될 수 있기에 프로그램에서 변수값 추적이 어려울 수 있음
- 값을 확인하기 전에는 타입을 확신할 수 없다.
- 동적 타입 언어는 유연성은 높지만 신뢰성은 떨어짐

<aside>
💡 변수를 사용할 때 주의사항

</aside>

- 변수는 꼭 필요한 경우에 한해 제한저긍로 사용한다.
- 변수의 유효범위를 좁게 만들어 부작용을 억제 한다.
- 전역 변수는 최대한 사용하지 않는다.
- 변수보다는 상수를 사용해 값의 변경을 억제한다.
- 이름은 정말 심사숙고해서 지어야 한다.

<aside>
💡 코드는 개발자를 위한 문서이기도 하다. 따라서 이해할 수 있게 가독성이 좋아야한다.

</aside>

> 컴퓨터가 이해하는 코드는 어떤 바보도 쓸 수 있다. 하지만 훌륭한 프로그래머는 사람이 이해할 수 있는 코드를 쓴다. 
- 마틴 파울러, <리팩토링>의 저자
>