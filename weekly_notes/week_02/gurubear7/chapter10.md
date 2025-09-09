# 10장

## 객체

원시 값을 제외한 나머지 값(함수, 배열, 정규표현식 등)

원시타입은 단 하나의 값 vs 객체 타입은 다양한 타입 값을 하나의 단위로 구성

객체는 프로퍼티(프로퍼티 키, 프로퍼티 값)로 구성.

프로퍼티는 key: value

함수도 프로퍼티라서 값으로 가능

다만 이때는 메서드라고 함

```jsx
var counter = {
	num: 0,
	increase: function () {
		this.num++;
	}
}
```

프로퍼티: 객체의 상태를 나타내는 data

메서드: 프로퍼티를 참조하고 조작하는 동작

함수와 객체는 밀접

객체의 집합으로 프로그램을 표현하는 패러다임 → 객체지향 프로그래밍

클래스 기반 객체지향 언어 vs 프로토타입 기반 객체지향 언어

- 클래스 기반
    - 클래스를 사전 선언 후, 필요한 시점에 인스턴스(메모리에 저장된 실체)를 생성
- 프로토타입 기반
    - 다양한 방법으로 생성
        - 객체리터럴
        - Object 생성자 함수
        - 생성자 함수
        - Object.create 메서드
        - 클래스

---

## 프로퍼티

객체는 프로퍼티의 집합

키(모든 문자열, symbol), 밸류(모든 값)

키 - 식별자 네이밍 규칙 지키면 그냥 써도 알아서, 규칙 안지키면 ‘’ 문자열로 만들어야 함

```jsx
var obj = {};
var key = 'hello';

obj[key] = 'world';

// 동적으로 넣기 가능
```

프로퍼티 키가 중복되면, 나중걸로 덮어씀 조심

---

## 메서드

객체에 묶여있는 함수

```jsx
var circle = {
	radius: 5,
	getDiameter: function () {
		return 2 * this.radius;
	}
};

console.log(circle.getDiameter()); // 10
```

객체자신.함수 로 호출 가능

---

## 프로퍼티 접근

마침표 표기법, 대괄호 표기법

[person.name](http://person.name) or person[’name’]

---

## 프로퍼티 값 갱신

같은 키에 값을 다시 할당해서 갱신

---

## 프로퍼티 동적 생성

없는 키에 값을 할당하여 생성

---

## 프로퍼티 삭제

```jsx
delete person.age; //삭제
// 존재하지 않는 걸 없애도 에러는 안남
```

---

## 추가된 객체 리터럴 확장 기능

```jsx
var x = 1, y = 2;

var obj = {
	x: x,
	y: y
};
// 식별자 표현식을 넣을 수 있음

const obj = { x, y };
// 이렇게 축약 가능
```

```jsx
const prefix = 'prop';
let i = 0;

const obj = {
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
	[`${prefix}-${++i}`]: i,
};
// 동적 생성 가능
```

---

## 메서드 축약 표현

```jsx
var circle = {
	radius: 5,
	getDiameter() {
		return 2 * this.radius;
	}
};

console.log(circle.getDiameter()); // 10

// 이렇게 메서드 축약 표현도 가능
```
