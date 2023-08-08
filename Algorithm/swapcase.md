# 대소문자 바꾸기( swapcase() )

### (1)

```python
import sys
input = sys.stdin.readline

S = input().rstrip()
result = ''

for i in S:
	if i.islower():
		result += i.upper()
	elif i.isupper():
		result += i.lower()
print(result)
```

- islower()과 isupper() 메소드를 이용해 대소문자 판별
- upper()과 lower() 메소드를 이용해 대소문자를 변경
- result 출력

### (2)

```python
import sys
input = sys.stdin.readline

S = input().rstrip()
# swapcase()는 문자열의 대문자를 소문자로, 소문자를 대문자로 변경
print(S.swapcase())
```

## TIP

Python으로 코딩테스트를 준비할 경우 내장함수를 적극활용하고 공부하자!
