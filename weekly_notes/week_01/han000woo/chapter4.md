> 용어에 대한 정확한 이해는 매우 중요한 역할을 한다.
이번 장에서는 앞으로 자주 사용할 용어의 의미를 주의 깊게 살펴보자.
> 

# 5.1 값

<aside>
💡 값(Value)은 식(표현식 expression)이 평가 (evaluate) 되어 생성된 결과

</aside>

<aside>
💡 평가란 식을 해석해서 값을 생성하거나 참조하는 것을 의미

</aside>

모든 값은 데이터 타입을 가지며, 메모리에 2진수로 저장된다 

2진수 0100 0001을 숫자로 해석할 경우 65지만 문자로 해석하면 ‘A’이다. 

### 값에 대한 예제

```jsx
// 1. 평가 수행 : 30 + 35
// 2. 할당 수행 : sum <- 65
var sum = 30 + 35;
console.log(sum); // 65
console.log(String.fromCodePoint(sum)); //"A"
```

# 5.2 리터럴

<aside>
💡 리터럴은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법

</aside>

![image.png](attachment:fc735d74-dd53-4c09-aa67-c523abef57c0:image.png)

# 5.3 표현식

<aside>
💡 표현식은 값으로 평가될 수 있는 문(statement)이다.     (표현식 → 값)
즉, 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.

</aside>

리터럴도 표현식이다.

<aside>
💡 표현식은 곧 값 혹은 
값이 될 수 있는 코드 라고 생각을 했다.

</aside>

```jsx
// 1. 리터럴 표현식 (값 자체가 표현식)
10;            // 숫자 리터럴 → 값 10
'hello';       // 문자열 리터럴 → 값 "hello"
true;          // 불리언 리터럴 → 값 true

// 2. 식별자 표현식 (변수나 객체 프로퍼티 참조)
let sum = 30;
let person = { name: "철수" };
let arr = [10, 20, 30];

sum;           // 변수 참조 → 값 30
person.name;   // 객체 프로퍼티 참조 → "철수"
arr[1];        // 배열 원소 접근 → 20

// 3. 연산자 표현식 (연산 결과가 값으로 평가됨)
10 + 20;       // 30
sum = 10;      // 대입 → 10 여기서 sum과 10은 동치
sum !== 10;    // false

// 4. 함수/메서드 호출 표현식 (호출 결과가 값)
function square(x) { return x * x; }

square(5);          // 25
person.getName = function() { return this.name; };
person.getName();   // "철수"
```

# 5.4 문 (statement)

<aside>
💡 문은 프로그램을 구성하는 기본 단위이자 최소 실행 단위다.

</aside>

또한 문은 여러 토큰으로 구성된다.

![image.png](attachment:e551a369-d176-427c-86ac-3927649ac28b:image.png)

# 5.5 세미콜론과 세미콜론 자동 삽입 기능

자바스크립트 엔진에는 세미콜론 자동 삽입 기능이 있다. 

문의 끝이라고 예측되는 지점에 세미콜론을 자동으로 붙여주는 기능이다. 

### ASI 차이 예제

```jsx
function text() {
	return //여기서 개행 시 ASI는 return;으로 인식한다.
    {
    	text : '가나다'
    }
}

console.log(text())
-------------------------------------------------------------------------------
$ node index.js 
undefined
```

```jsx
function text() {
	return {
    	text : '가나다'
    }
}

console.log(text())
-------------------------------------------------------------------------------
$ node index.js 
{ text: '가나다' }
```

https://ordinary-code.tistory.com/149

# 5.6 표현식인 문과 표현식이 아닌 문

```jsx
var x; //변수 선언문은 값으로 평가될 수 없으므로 표현식이 아니다.
x = 1 + 2; // 표현식이면서 완전한 문이기도 하다.
```

<aside>
💡 표현식인 문과 표현식이 아닌 문을 구별하는 가장 간단하고 명료한 방법은 변수에 할당해 보는것

</aside>

```jsx
var foo = x = 100;
// x = 100 은 표현식이기에 값으로 치환이 가능하다.
console.log(foo); // 100
```

<aside>
💡 크롬 개발자 도구에서는 표현식인 문은 값을 출력하고 아니면 undefined를 출력한다.

</aside>