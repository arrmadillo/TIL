# 스택(Stack) 자료구조

> 먼저 들어 온 데이터가 나중에 나가는 형식(선입후출)의 자료구조

스택 구현 예제

```python
stack = []
stack.append(1)
stack.append(2)
stack.append(3)
stack.append(4)
stack.pop()
stack.append(5)
print(stack) # 최하단 원소부터 출력
# [1, 2, 3, 5]
print(stack[::-1]) #최상위 원소부터 출력
# [5, 3, 2, 1]
```

<img src='img/stack.png' width=50% height=50%/>

# 큐(Queue) 자료구조

먼저 들어 온 데이터가 먼저 나가는 형식(선입선출)의 자료구조

```python
from collections import deque
# 큐 구현을 위해 deque 라이브러리 사용

queue = deque()

queue.append(1)
queue.append(2)
queue.append(3)
queue.append(4)
queue.popleft()
queue.append(7)

print(queue) # 먼저 들어온 순서대로
# deque([2, 3, 4, 7])
queue.reverse() # 역순으로 바꾸기
print(queue)
# deque([7, 4, 3, 2])
```
