# 15장

## let, const와 블록 레벨 스코프

### var 의 문제점

- 변수 중복 선언 허용
- 함수레벨 스코프
    - var 은 함수의 코드블록{}만 지역 스코프로 인정
    - if, for 문에서 쓴 것은 전역 변수가 되어버림
- 변수 호이스팅
    - 호이스팅이 돼서 선언문 이전에 참조가 가능한데,
    - 이는 가독성 떨구고 예기치 못한 오류 가능

### let 키워드

- 변수 중복 선언 금지
    - let은 중복 선언하면 에러
- 블록레벨 스코프
    - {} 코드블록 모두를 지역 스코프로 인정
- 변수 호이스팅
    - let에서는 변수 호이스팅이 발생하지 않는 것처럼 동작
    - var 는 선언과 초기화 동시에. let은 선언만하고 초기화 되어있지 않음

- 호이스팅이 발생하긴 함
    
    ```jsx
    let foo = 1;
    
    {
    	console.log(foo); // ReferenceError
    	let foo = 2; // 지역변수
    }
    ```
    
    - 호이스팅이 발생하지 않는다면, 에러가 아니고 전역변수를 호출해야함

- 전역 객체와 let
    
    ```jsx
    var x = 1;
    
    console.log(window.x); // 1
    ```
    
    ```jsx
    let x = 1;
    
    console.log(window.x); // undefined
    ```
    

### const 키워드

상수를 선언하기 위해 사용. 상수 이외에도 쓰긴 함

- const 는 반드시 선언과 동시에 초기화 해야 함
- 선언하고 초기화가 분리된다면, 이미 재할당을 하는거니까 위배됨

```jsx
const foo; //에러
const foo = 1;
```

- 재할당 금지

```jsx
const foo = 1;
foo = 2; // 에러
```

- 상수 - 세율, rate 류
    - 고정된 값 사용
    - 의미를 쉽게 알 수 있도록
    - 나중에 세율이 변경되면 상수만 변경하면 전체 반영 돼서 좋음
    - 대문자 스네이크로 표현
    
    ```jsx
    const TAX_RATE = 0.1;
    ```
    

### const 키워드와 객체

```jsx
const person = {
	name: 'Lee'
};

person.name = 'Kim'; // const 는 재할당 불가,
// 그렇지만, 객체는 변경가능

// 이건 재할당 이니까 불가
person = {
	name: 'Kim'
}

// 이건 let, const 중복 선언 불가로 불가
const person = {
	name: 'Kim'
}

```

### var vs. let vs. const

- 기본 const 를 쓰고, 재할당 필요한 것만 let 을 쓴다고 생각하자
- var 은 쓰지 않음
- let 쓰는 경우 스코프는 최대한 좁게
- 변경 없고, 읽기 전용인 값은 const 사용
- 잘 모르면 const 하고, 하다가 재할당 필요한거 같으면 let으로 변경
