# 9장

## 타입 변환

### 명시적 타입 변환(타입 캐스팅)

개발자가 의도적으로 값의 타입 변경

```jsx
var x = 10;

var str = x.toString();
console.log(typeof str, str);
```

### 암묵적 타입 변환

- 
    - 연산자는 하나라도 문자열이면 문자열로 변환
    - '10' + 2 → 숫자가 문자열로 '102'
    - 스킬: -0 + '' → '0' 과 같이 공백 문자열을 붙여서 문자로 바꾸는 스킬
- -, *, / 연산자는 숫자 타입으로 변환
    - 1 - '1' → 0
    - 1 * '10' → 10
    - 1 / 'one' → NaN
    - 5 * 'String' → NaN
- 비교연산자
    - '1' > 0 → true
- 단항 +
    - 숫자로 바꾸려는 시도. 안되면 NaN
    - +[] 는 0 조심
    - 나머지는 숫자 안될 거 같으면 다 NaN 이다

### 불리언 변환

조건식에서 false로 평가 되는 Falsy 값

- false
- undefined
- null
- 0, -0
- NaN
- ''

---

## 명시적 타입 변환

### 문자 타입

- String(1), String(NaN)
- (1).toString()
- 1 + ''

### 숫자 타입

- Number('0'), Number(true)
- parseInt('0'), parseFloat('10.53')
- +'0', +true
- '0' * 1

### 불리언 타입

- Boolean('x'), Boolean(''), Boolean(undefined)
- Boolean([]) - 이거 true임. 조심
- !!'x' → true
- !!'' → false
- 이거 간편해서 자주 쓸 듯 - !!

---

## 단축 평가

표현식을 평가하는 도중에 확정나면 나머지 생략.

타입 변환하지 않고 그 값 그대로 반환

- 'Cat' && 'Dog' → 'Dog'
    - Cat 은 true, Dog를 확인해서 Dog 반환
- 'Cat' || 'Dog' → 'Cat'
    - 왼쪽거 평가해서 Truthy니까 바로 Cat 반환

### 활용 예시

- 객체의 프로퍼티 참조할 때 에러 방지
    
    ```jsx
    var elem = null;
    var value = elem && elem.value; // elem.value 만을 어디에 할당하면 에러, 여기선 null
    ```
    
- 함수 매개변수 기본값 설정
    
    ```jsx
    function getStringLength(str) {
    	str = str || '';
    	return str.length
    }
    ```
    
- 간단한 조건부 실행
    - 예: isLoggedIn && renderComponent() → 자주 쓰임 (리액트에서 특히 흔함)

---

## 옵셔널 체이닝 연산자

?.

- 에러 대신 undefined 내고 싶을떄

```jsx
var elem = null;

var value = elem?.value; //elem.value는 에러, 여기서는 undefined
```

- str = '' 같이 falsy라도, 참조 이어가고 싶을 때
    
    ```jsx
    var str = '';
    
    var length = str?.length; // str && str.length는 '' 반환, 여기선 0
    // str = null 인 경우 원래는 에러인데 이렇게 하면 undefined
    ```
    

---

## null 병합 연산자

기본값 설정할 때.

원래는 이렇게 했는데,

```jsx
var name = userInput || "Guest" // userInput이 '', 0 이어도 Guest를 넣음
```

이렇게 쓰면,

```jsx
var name = userInput ?? "Guest" // userInput이 null, undefined인 경우만 오른쪽 넣음
```
