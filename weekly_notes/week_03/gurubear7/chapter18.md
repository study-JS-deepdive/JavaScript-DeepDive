### 일급객체

1. 무명의 리터럴로 생성 가능. 런타임에 생성
2. 변수나 자료구조에 저장
3. 함수의 매개변수에 전달
4. 함수의 반환값으로 사용가능

```jsx
// 1. 무명의 리터럴 
// 2. 변수에 저장

const increase = function (num) {
	return ++num;
};

const decrease = function (num) {
	return --num;
};

// 2. 객체에 저장
const auxs = { increase, decrease };

// 3. 매개변수에 전달
// 4. 반환값으로 사용
function makeCounter(aux) {
	let num = 0;
	
	return function () {
		num = aux(num);
		return num;
	};
}

// 3. 매개변수 전달
const increaser = makeCounter(auxs.increase);
increaser();
```

- 일급 객체라는 것 = 함수를 객체와 동일하게 사용할 수 있다는 말
- 값을 사용할 수 있는 곳이라면, 모두 리터럴로 정의 가능
- 매개변수, 반환값으로 사용 → 함수형 프로그래밍 가능
- 그런데, 일반 객체와 달리 함수는 호출 가능

### 함수 객체의 프로퍼티

객체이기 때문에 프로퍼티 가짐

프로퍼티 중 함수인 것 → 메서드라고 부름

- length, name, arguments, caller, prototype

- **arguments**
    - 전달된 인수들 정보 가짐, 개수 부족하면 남은거 undefined, 초과되면 무시 but arguments 가 가지고 있음
    
    ```jsx
    // 이런거 가능. 인수 몇개가 들어오든 처리할 수 있다
    function sum() {
    	let res = 0;
    	
    	for (let i = 0; i < arguments.length; i++) {
    		res += arguments[i];
    	}
    	return res;
    }
    ```
    
    - 유사 배열 객체 - 배열이 아니지만 이터러블 가능
    - 근데 배열에서 할 수 있는 작업 못하니까 Rest 파라미터 도입
    
    ```jsx
    // 받는 인수들을 ...args 로 처리해서 배열만듬
    function sum(...args) {
    	return args.reduce((pre, cur) => pre + cur, 0);
    }
    ```
    

- **caller**
    - 비표준. 별로 안중요. 나 자신을 호출한 함수가 뭔지 알려줌

- **length**
    - 매개변수의 개수
    - arguments.length 와 구분
    - arguments.length = 인자의 개수(호출 시 실제로 넘어온 것), 객체.length = 매개변수의 개수(몇개를 받기로 정해진 것)
- **name**
    - 함수 이름
    - ES6에서는 함수 객체를 가리키는 변수 이름으로.

- “__proto__” 접근자 프로퍼티
    - 모든 객체는 [[Prototype]] 이라는 내부 슬롯은 가짐.
    - 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체
    - 여기에 접근하기 위해 쓰는 접근자
    
- **prototype**
    - constructor 만이 소유하는 프로퍼티
    - 일반 객체는 없고, 함수만 있다

- 생성자 함수 vs class
    - 예전엔 생성자 함수로 객체를 생성했었는데,
    - 요즘엔 class 선호
    
    ```jsx
    // 생성자
    function Person(name, age) {
      this.name = name;
      this.age = age;
    }
    
    Person.prototype.sayHello = function() {
      console.log(`Hi, I'm ${this.name}`);
    };
    
    // 클래스
    class Person {
      constructor(name, age) {
        this.name = name;
        this.age = age;
      }
    
      sayHello() {
        console.log(`Hi, I'm ${this.name}`);
      }
    }
    ```
