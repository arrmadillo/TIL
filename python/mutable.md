# call by value / call by reference

# (int)다음 실행 결과를 예측해보자.

```python
def swap(a, b):
    a, b = b, a
    print(a, b)

n, m = 10, 20
swap(n, m)
print(n, m)
```

# 결과

```python
20 10
10 20
```

- swap 함수가 호출
- a, b 값이 서로 바뀌고 20 10
- 프린트 문으로 n, m 값 10 20 출력

---

### 왜 이런 결과가 나올까?

파이썬에서는 변수 type에 따라 **immutable**과 **mutable**한 자료형이 있다.

자료 값을 변경할 수 있는가? 라는 기준이다

변경 가능한 type은 **mutable** 자료형 그렇지 않은 자료형을 **immutable** 자료형이다.

### 대표적인 **immutable** 자료형

- int
- string
- tuple
- bool

### 대표적인 **mutable 자료형**

- list
- dict

### immutable 자료형(call by value - 값으로 호출)

- 변수에 값이 직접 저장되는 것이 아니라 값이 저장된 주소를 참조한다.

```python
a = 99
b = 99

a와 b는 같은 메모리 주소를 참조 한다.
**값이 바뀌면 그 값에 대한 메모리 주소를 참조한다.**
```

### mutable 자료형(call by reference - 참조로 호출)

- 변수마다 각각 메모리 주소가 생성 된다.

```python
a = [1, 2, 3]
b = [1, 2, 3]

같은 리스트 값을 가지고 있지만 a, b의 주소는 다르다.
**값이 바뀌어도 메모리 주소는 바뀌지 않는다.**
```

# 결과 이유

- immutable 자료형인 int 변수가 함수 인자로 넘어감
- 변수가 그대로 넘어가는 것이 아닌 **변수가 갖고 있던 값을 복사**하여 넘김
- 함수를 빠져나온뒤 변수 값은 변하지 않음

---

# (list)다음 실행 결과를 예측해보자.

```python
def modify(arr):
    arr[0] = 5

nums = [1, 2, 3, 4]
modify(nums)

for num in nums:
    print(num, end=" ")
```

# 결과

```python
10 2 3 4
```

# 결과 이유

- 리스트는 mutable하기 때문에 **변수 자체의 주소**가 넘어감
- 변수 값이 수정됨
