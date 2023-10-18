# 조건문(IF / 삼항연산자)

```jsx
let number = 3;

if (number === 1) {
  console.log('1');
} else if (number === 2) {
  console.log('2');
} else {
  console.log('1도 아니고 2도 아닙니다.'); 
}

// 삼항연산자
number === 3 ? console.log('3입니다.') : console.log('3이 아닙니다.');

let value = '홍길동';
let name = value === '홍길동' ? '홍길동' : '아무개';

console.log(name);
```

## 출력

1도 아니고 2도 아닙니다.
3입니다.
홍길동