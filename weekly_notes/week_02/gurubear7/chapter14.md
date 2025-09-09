# 14장

## 전역 변수의 문제점

전역 변수는 피하자

### 변수 생명주기

생성되고 값을 갖고, 소멸하는 주기

```jsx
function foo() {
	var x = 'local';
	console.log(x); // local
	return x;
} // -> x 변수 소멸

foo(); // local print, 'local' 반환
console.log(x); // ReferenceError
```

- 그런데, 누군가 스코프를 참조하고 있다면 해제되지 않음

```jsx
var x = 'global';

function foo() {
	console.log(x); // local
	var x = 'local';
}

foo();
console.log(x); // global
```

호이스팅

변수 선언이 스코프의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유 특징

### 전역변수 생명주기

- 함수는 반환문에서 종료되는데,
- 전역 코드에는 없으니까 마지막 문 실행으로 더 이상 실행할 문 없으면 종료

### 전역 객체

코드가 실행되기 이전에 어떤 객체보다도 먼저 생성되는 객체

- 브라우저 - window
- 서버사이드(node) - global
- 식별자 globalThis 로 통일

### 전역변수 문제점

- 암묵적 결합
    - 모든 코드가 변경 가능. 가독성 낮고, 의도치 않게 변경
- 긴 생명주기
    - 메모리 리소스를 오랫동안 점유
    - 특히 var 은 변수 이름 중복 선언이 가능하고, 값 재할당 가능
- 스코프 체인 상에서 종점에 존재
    - 가장 마지막에 검색 - 검색 속도 느림
- 네임스페이스 오염
    - 파일이 달라도 하나의 전역 스코프 공유하게 됨

### 전역변수 사용 억제하는 방법

변수 스코프는 좁을수록 좋다

사용 억제 방법

- 즉시실행함수
    - 즉시 실행 후 소멸 되도록
- 네임스페이스 객체
    
    ```jsx
    var MYAPP = {};
    
    MYAPP.name = 'Lee';
    
    console.log(MYAPP.name);
    ```
    
    → 어차피 객체를 전역에 선언하므로 마찬가지임
    
- 모듈패턴
    - 클로저를 활용하여 전역 변수의 억제, 캡슐화 구현
    - 객체의 상태와 메서드를 하나로 묶음 (캡슐화→정보은닉)
    
    ```jsx
    var Counter = (function () {
    	var num = 0;
    	return {
    	increase() {
    		return ++num;
    	},
    	decrease() {
    		return --num;
    	}
    	};
    }());
    
    console.log(Counter.num); // undefined 외부에 공개 안함
    console.log(Counter.increase()); // 1 외부에 공개
    ```
    
    ```jsx
    // 위 문법은 이걸 축약 메서드 문법으로 쓴거임
    {
      increase: function() {
        return ++num;
      },
      decrease: function() {
        return --num;
      }
    }
    ```
