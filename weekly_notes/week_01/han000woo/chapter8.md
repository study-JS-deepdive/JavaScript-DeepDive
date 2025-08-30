- 코드 블록에 제어를 위해 사용(조건 분기, 반복 실행)
- if, for문 외에도 forEach, map, filter, reduce등이 있음

# 8.1 블록문

- 0개 이상의 문을 중괄호로 묶은 것
- 문의 끝에는 세미콜론을 붙이지만 블록문은 자체 종결서을 갖기에 세미콜론을 붙이지 않는다

```jsx
// 단순 블록문
{
  let x = 10;
  let y = 20;
  console.log("x + y =", x + y);
}
```

# 8.2 조건문

- 조건식의 평가 결과에 따라 코드 블록을 결정한다
- if else와 switch가 있다

## 8.2.1 if… else문

- 조건식을 추가하여 코드블록을 늘리고 싶을땐 else if를 사용한다
- if와 else는 2번 이상 사용할 수 없다. 다만 else if는 여러 번 사용할 수 있다.
- else if와 else는 옵션이다.

```jsx
let score = 85;

if (score >= 90) {
  console.log("A 등급 🎉");
} else if (score >= 80) {
  console.log("B 등급 👍");
} else if (score >= 70) {
  console.log("C 등급 🙂");
} else if (score >= 60) {
  console.log("D 등급 😅");
} else {
  console.log("F 등급 ❌");
}

```

- 삼항 연산자는 표현식이지만 if else는 표현식으로 사용될 수 없다.

```jsx
// if else의 경우
let age = 20;
let message;

if (age >= 18) {
  message = "성인입니다. 🎉";
} else {
  message = "미성년자입니다. ❌";
}

// 삼항 연산자의 경우 
let message = age >= 18 ? "성인입니다. 🎉" : "미성년자입니다. ❌";
```

## 8.2.2 switch 문

- 여러 케이스를 묶어서 사용할 수도 있다.

```jsx
let fruit = "banana";

switch (fruit) {
  case "apple":
  case "pear":
    console.log("사과 또는 배 🍎🍐");
    break;
  case "banana":
  case "mango":
    console.log("바나나 또는 망고 🍌🥭");
    break;
  default:
    console.log("알 수 없는 과일입니다.");
}
```

# 8.3 반복문

## 8.3.1 for문

- for (변수 초기화; 조건식; 실행, 증감문)으로 구성
- 아무것도 없을 경우 무한 루프 for(;;)

```jsx
// 구구단 예제
for (let i = 2; i <= 9; i++) {
  console.log(`📌 ${i}단`);
  
  for (let j = 1; j <= 9; j++) {
    console.log(`${i} x ${j} = ${i * j}`);
  }
}
```

## 8.3.2 while문

- 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속 실행한다

```jsx
// 1부터 5까지 출력
let i = 1;

while (i <= 5) {
  console.log("i =", i);
  i++;
}
```

## 8.3.3 do … while 문

- 코드블록을 먼저 실행하고 조건식을 평가한다.

```jsx
// 1부터 5까지 출력
let j = 1;

do {
  console.log("j =", j);
  j++;
} while (j <= 5);
```

# 8.4 break 문

- 레이블 문, 반복문, switch문의 코드 블록을 탈출한다
- 이 외에 코드 블록에 사용시 SyntaxEror 발생

<aside>
💡

레이블 문이란 식별자가 붙은 문을 말한다.

</aside>

```jsx
outerLoop: for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    console.log(`i=${i}, j=${j}`);

    if (j === 2) {
      console.log("👉 바깥 반복문(outerLoop) 종료");
      break outerLoop; // 바깥 for문까지 종료
    }
  }
}

-------------------------------------------------------------------
i=1, j=1
i=1, j=2
👉 바깥 반복문(outerLoop) 종료
```

# 8.5 continue 문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고 다음 증감식으로 이동시킨다.

```jsx
outerLoop: for (let i = 1; i <= 3; i++) {
  for (let j = 1; j <= 3; j++) {
    if (j === 2) {
      console.log(`j가 2일 때 바깥 반복문(${i}단계) 다음으로 건너뜀`);
      continue outerLoop; // 안쪽 루프 건너뛰고 바깥 루프 증감으로 이동
    }
    console.log(`i=${i}, j=${j}`);
  }
}
-----------------------------------------------------------------------
i=1, j=1
j가 2일 때 바깥 반복문(1단계) 다음으로 건너뜀
i=2, j=1
j가 2일 때 바깥 반복문(2단계) 다음으로 건너뜀
i=3, j=1
j가 2일 때 바깥 반복문(3단계) 다음으로 건너뜀
```