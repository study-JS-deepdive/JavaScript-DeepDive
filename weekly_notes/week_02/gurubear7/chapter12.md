# 12장

## 함수

입력을 받아 출력을 내보내는 일련의 과정

### 함수 사용 이유

동일한 작업을 정의해놓고, 필요할 때 반복 재사용 가능

### 함수 리터럴

함수는 객체 타입의 값. 리터럴로 생성 가능

함수는 호출 가능, 일반 객체는 불가

함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자, 외부에서 이름으로 참조 불가

add(1+2);

### 함수 정의

- 함수 선언문

```jsx
function add(x, y){
	return x + y;
}
```

- 함수 표현식 - 전체가 표현식인가?

```jsx
var add = function (x, y){
	return x + y;
}
// undefined 인데 이게 왜 표현식?

var score = 100; // undefined -> 표현식 아님

```

- Function 생성자

```jsx
var add = new Function('x', 'y', 'return x + y');

```

- 화살표 함수

```jsx
var add = (x, y) => x + y;
```

### 호이스팅

- 함수 선언문 으로 하는 경우 → 함수 객체로 초기화
- 함수 표현식 으로 하는 경우 → 변수 선언문 처럼 undefined로 초기화 (변수 호이스팅)

→ 웬만하면 함수 표현식을 쓰자 !

Function 생성자 함수

```jsx
var add = new Function('x', 'y', 'return x + y');
```

→ 하지말자

### 화살표 함수

```jsx
const add = (x, y) => x + y;
```

### 함수 호출

- 매개변수는 함수 내부에서만 참조 가능
- 개수 안맞아도 호출 되는데, 대신 인수를 덜넣은 경우, 매칭 없는 애는 undefined
- 인수 개수 초과하는 경우, 무시 → 버려지진않고 arguments로 보관
- 타입도 지정 안됨

```jsx
function add(x, y) {
  if (typeof x !== 'number' || typeof y !== 'number') {
    // 매개변수를 통해 전달된 인수의 타입이 부적절할 경우 에러를 발생시킨다.
    throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
  }

  return x + y;
}

console.log(add(2));       // TypeError: 인수는 모두 숫자 값이어야 합니다.
console.log(add('a', 'b')); // TypeError: 인수는 모두 숫자 값이어야 합니다.
```

→ 그냥 타입스크립트 하자..

- 매개변수 3개 넘지 않을 것 권장. 이상적인 함수는 한 가지 일만 해야한다.

### 반환문

- return 과 표현식으로 외부로 반환
- 반환문 이후에 오는건 무시
- return; 만 적으면 undefined 반환
- return 없을때 undefined 반환

### 참조에 의한 전달, 외부 상태 변경

```jsx
function changeVal(primitive, obj) { 
	primitive += 100;
	obj.name = 'kim';
}

var num = 100;
var person = { name: 'Lee' };

changeVal(num, person);
```

- 원시 타입인 num 은 immutable이라 바뀌지 않음.
- 객체는 mutable이므로 변경됨

### 즉시 실행 함수

```jsx
(function (){
	var x = 3;
	var y = 5;
	return x + y;
}('인자가 여기들어감'));
```

### 재귀함수

```jsx
function factorial(n){
	if (n <= 1) return 1;
	return n * factorial(n-1);
}

```

### 중첩함수

```jsx
function outer(){
	var x = 1;
	
	function inner(){
		var y = 2;
		console.log(x + y);
	}
	inner();
}
```

### 콜백함수

```jsx
function repeat(n, f){
	for (var i = 0; i < n; i++){
		f(i);
	}
}

var logAll = function (i) {
	console.log(i);
};

repeat(5, logAll);

var logOdds = function (i) {
	if (i % 2) console.log(i);
};
```

→ 매개변수를 통해 다른 함수의 내부로 전달되는 함수 logAll 콜백함수.

→ repeat은 고차함수.

### 콜백 함수(익명 함수 리터럴로) 활용 예시

```jsx
document.getElementById('myButton').addEventListener('click', function () {
	console.log('button clicked');
});

setTimeout(function () {
	console.log('1초 경과');
}, 1000);

```

### 순수함수, 비순수함수

순수함수 - 외부 상태를 변경하지 않고, 외부 상태에 의존하지 않는 함수

→ 순수함수를 쓰도록 하자 !
