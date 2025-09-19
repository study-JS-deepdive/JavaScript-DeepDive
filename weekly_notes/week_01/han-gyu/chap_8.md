### 8장 제어문

- 제어문
    - 조건에 따라 코드 블록을 실행, 반복실행 할 때 사용
    - 코드 실행을 인위적으로 제어 가능

---

### 8.1 블록문

- 0개 이상의 문을 중괄호로 묶은 것
- 코드 블록, 블록 이라 칭함
- 하나의 실행단위로 취급

```jsx
// 블록문
{ 
	var func = 10;	
}

// 제어문
var x = 1;
if(x < 10) {
 x++;
}

// 함수 선언문
function sum(a, b) {
	return a + b;
}

```

---

### 8.2 조건문

- 주어진 조건식의 평가 결과에 따라 코드 블록실행 결정

- **if else**
    - if…else / switch 문 두가지 조건문 제공

```jsx
if (조건식) {
 // 조건식이 참이면 실행
} else if(조건식2) {
 // 조건식2 참이면 실행
} else {
 // 조건식이 거짓이면 실행
}

```

- 조건에 따라 단순히 값을 결정하여 변수에 할당하는 경우, if else 보다 삼항 조건 연산자를 사용하는 편이 가독성이 좋다

- **switch**
    - 주어진 표현식을 평가, 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름 전환
    - 일치하는 항목이 없는 경우 default 로 이동

```jsx
switch(표현식) {

 case 표현식1:
  switch 문의 표현식과 표현식1이 일치하면 실행될 문;
  break;
 case 표현식2:
  switch 문의 표현식과 표현식2가 일치하면 실행될 문;
  break;
 default:
	switch 문의 표현식과 일치하는 case 문이 없을 때 실행될 문;	
} 
  
  
```

- if else 는 논리적 참 거짓으로 실행할 코드블록 결정
- switch 문은 다양항 case 에 따라 실행할 코드블록 결정

---

### 8.3 반복문

- 조건식의 평가 결과가 참인 경우 코드블록 실행
- 조건식이 거짓일 때까지 반복 실행
- for문, while 문, do.. while 문 제공

for 문

```jsx
for (변수 선언문, 할당문; 조건식; 증감식) {
	조건식이 참인 경우 반복 실행될 문;
}

for(var i = 0; i < 2; i++) {
 console.log(i);
}
```

```jsx
// 중첩된 for 문

for(var i = 1; i <= 6; i++) {
 for(var j = 1; j <= 6; j++) {
   if(i + j === 6) console.log(`[${i}], ${j}`);
 }
}
```

**while 문**

- 반복 횟수가 명확할 때 주로 사용
- while 문은 반복 횟수가 불명확할 때 주로 사용

```jsx
var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행
while (count < 3) {
 console.log(count);
 count++;
}

// 무한루프 for 문
while (true) {...}
```

**do .. while 문**

- 코드 블록을 먼저 실행하고 조건식 평가
- 코드 블록은 무조건 한 번 이상 실행된다.

```jsx
var count = 0;

// count 가 3보다 작을 때까지 코드 블록을 계속 반복 실행
do {

console.log(count);
 count++;
} while (count < 3); 
```

---

### 8.4 break 문

- **break 문은 코드 블록을 탈출한다.**

```jsx
if(true) {
 break;
}
```

---

### 8.5 continue 문

- 반복문의 코드 블랙 실행을 현 지점에서 중단, 반복문의 증감식으로 이동

 

```jsx
for(var i = 0; i < stirng.length; i++) {
  if(string[i] !== search ) continue;
  count++;
}
```
