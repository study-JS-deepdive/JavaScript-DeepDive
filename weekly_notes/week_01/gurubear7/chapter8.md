# 8장

# 제어문

위에서 아래로 순차적으로 코드가 실행되는데, 제어문으로 흐름을 인위적으로 제어 가능

---

# 블록문

중괄호로 묶어서 실행 단위를 취급하는 방법

```jsx
var x = 1;

// 제어문
if (x < 10) {
	x++;
}

// 함수 선언문
function sum(a, b) {
	return a + b;
}
```

---

# 조건문

조건식의 평가 결과에 따라 코드 블록의 실행을 결정

```jsx
var num = 2;
var kind;

if (num > 0) {
	kind = '양수';
} else if (num < 0){
	kind = '음수';
} else {
	kind = '영';
}
```

---

# switch 문

표현식을 평가하여 값이 일치하는 case 문을 실행한다. 없으면 default 문 실행

```jsx
var day = 6;
var dayName;

switch (day) {
	case 1: dayName = "Mon";
		break;
	case 2: dayName = "Tue";
		break;
	case 3: dayName = "Wed";
		break;
	case 4: dayName = "Thu";
		break;
	case 5: dayName = "Fri";
		break;
	case 6: dayName = "Sat";
		break;
	case 7: dayName = "Sun";
		break;
	default: dayName = 'Invalid day';
}
// break 안쓰면 fall through 발생 -> Invalid day 출력
// 쓰면 'Sat'

// fall through 를 의도적으로 이용하는 경우도 있음
case 4: case 6: case 9: case 11: // 한번에 처리하는 경우
```

→ if else로 가능하다면 if else, 조건이 너무 복잡할 때만 switch 고려

---

# 반복문

```jsx
for (var i = 0; i < 2; i++) {
	console.log(i);
}
```

![image.png](attachment:9397ea45-420b-4f30-afdf-a972250eb563:image.png)

---

# 이중 중첩 for문

```jsx
for (var i = 1; i <= 6; i++) {
	for (var j = 1; j <= 6; j++) {
		if (i + j === 6) console.log(`[${i}, ${j}]`);
	}
}
```

---

# while 문

반복 횟수가 불명확할 때 반복

조건문 평가 결과가 거짓이 되면 탈출

```jsx
var count = 0;

while (count < 3) {
	console.log(count);
	count++;
}

// 조건이 계속 true 면, 무한루프
while (true) { /* ... */ } // 이 안에 if (...) + break 를 넣어서 탈출 조건 
```

---

# do … while 문

일단 한번은 실행하고 반복하고 싶은 경우

```jsx
var count = 0;

do {
	console.log(count);
	count++;
} while (count < 3);
```

---

# break 문

레이블 문, 반복문, switch 문 코드 블록 탈출

- 레이블 문 - 식별자가 붙은 문

```jsx
foo: console.log('foo');

foo: {
	console.log(1);
	break foo; // foo 문 탈출
	console.log(2);
}
```

이중 for 문에서 안쪽만 break가 아닌, 바깥쪽까지 탈출하고 싶을때

```jsx
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출한다.
    if (i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log('Done!');
```

---

# continue 문

코드블록 실행을 중단하고, 다음 차례로 넘어가도록 하는 것( break는 안 넘어가고 끝냄)

```jsx
var string = 'Hello World.';
var search = 'l';
var count = 0;

for (var i = 0; i < string.length; i++) {
  if (string[i] !== search) continue; // count하지 않고, 다음으로
  count++; 
}

console.log(count); // 3

const regexp = new RegExp(search, 'g');
console.log(string.match(regexp).length); // 3
```

if 문을 간결하게 적을 필요가 있을 때, continue 쓰고 if 문 밖에 코드를 적을 수 있다.
