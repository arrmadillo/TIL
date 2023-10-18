# 반복문(while)

```jsx
// 조건이 false가 될때까지 실행

let num = 0;

while (true) {
  if (num === 10) {
    console.log('끝!');
    break;
  }
  console.log(num);
  num++;
}
```

## 출력

0
1
2
3
4
5
6
7
8
9
끝!

---

## do while

```jsx
// 조건이 false가 될때까지 실행

let num = 0;

do { // do 부분부터 먼저 실행하고 아래 while 조건 확인
  console.log('출력!');
  console.log(num);
  num++;
} while (num < 5);
```

## 출력

시작!
0
시작!
1
시작!
2
시작!
3
시작!
4