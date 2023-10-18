# 반복문(for)

```jsx
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

## 출력

0
1
2
3
4

---

# break

```jsx
for (let i = 1; i < 20; i++) {
  if (i % 2 === 0) {
    console.log(`${i}는 짝수입니다.`);
    break; // 반복문 종료
  } else {
    console.log(i);
  }
}
```

## 출력

1
2는 짝수입니다.

# continue

```jsx
for (let i = 1; i < 10; i++) {
  if (i % 2 === 0) {
    continue; // 그 다음 반복으로 넘어감
  } else {
    console.log(i);
  }
}
```

## 출력

1
3
5
7
9