# 함수

## 함수 기본

```jsx
function add(a, b) {
  // 함수선언
  const result = a + b;
  return result;
}

// 함수호출
console.log(add(1, 2));
```

- return을 사용하지 않으면 **undefined**이 반환됨
- return을 하게 되면 함수가 종료됨. 즉 return이 실행되면 return 아래의 코드들을 실행불가

## 매개변수

- 매개변수의 기본값은 **undefined**
- 매개변수에 기본값을 지정 가능
    
    ```jsx
    function add(a = 1, b = 2) { // 기본값 지정
      console.log(a + b);
    }
    
    add();
    // 5출력
    ```
    

- 기본값을 지정한  매개변수를 뒤쪽에 위치해야함

```jsx
function add(a = 2, b) {
  console.log(a);
  console.log(b);
  console.log(a + b);
}

add(3);

// 출력
3
undefined
NaN
```

- 함수에 매개변수를 여러개 전달하기. ****Rest파라미터****

```jsx
function sum(a, b, ...numbers) {
  console.log(a);
  console.log(b);
  console.log(numbers);
}

sum(1, 2, 3, 4, '테스트');

출력
1
2
[ 3, 4, '테스트' ]
```

## 함수 표현식

```jsx
//함수 선언문
function add(a, b) {
  return a + b;
}

// 함수 표현식
let add2 = function (a, b) {
  return a + b;
};

// 화살표 함수
add2 = (a, b) => {
  return a + b;
};

// a + b 리턴
add2 = (a, b) => a + b;
```

## 콜백함수

```jsx
add = (a, b) => a + b;
multiply = (a, b) => a * b;

function calculator(a, b, action) {
  let result = action(a, b);
  console.log(result);
  return result;
}

calculator(1, 2, add);
calculator(2, 4, multiply);
```