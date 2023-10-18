# 스위치(switch)

```jsx
const day = 0;
let value;

switch (day) {
  case 0:
    value = '월';
    break;
  case 1:
    value = '화';
    break;
  case 2:
    value = '수';
    break;
  case 3:
    value = '목';
    break;
  case 4:
    value = '금';
    break; // 브레이크를 항상 적어야 한다. 그렇지 않으면 아래 케이스를 확인함.
}

console.log(value);
```

## 출력

월

---

```jsx
let grade = 3;

switch (grade) {
  case 3:
  case 2:
    console.log('좋은 성적이네요');
    break;
  case 1:
    console.log('나쁘지 않아요');
    break;
  default:
    console.log('성적을 다시 입력하세요');
}
```

## 출력

좋은 성적이네요

## default는 위에 해당하는 케이스가 없을시 실행