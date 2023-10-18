# 단어필터

```jsx
입력:
5 12
goorm
goormabgoorm
```

```jsx
출력: ab;
```

입력한 단어를 없애는 문제이고 모든 단어가 삭제되어서 출력이 없을땐 EMPTY 출력

## 정답 1

```python
import sys
input = sys.stdin.readline
N, M = map(int, input().split())
S = input().rstrip()
E = input().rstrip()

while S in E:
    idx = E.find(S)
    for _ in range(len(S)):
        E = E[:idx] + E[idx + 1:]

if E:
    print(E)
else:
    print("EMPTY")
```

## 정답 2

```python
import sys
input = sys.stdin.readline
N, M = map(int, input().split())
S = input().rstrip()
E = input().rstrip()

# 필터 단어 S가 메세지 E에 포함시 코드를 계속 실행
while S in E:
    # replace 메소드를 통해서 E에서 S 단어를 계속 삭제
    E = E.replace(S, '')
if E:
    print(E)
else:
    print("EMPTY")
```
