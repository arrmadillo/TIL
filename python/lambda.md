# 람다 함수

> 함수를 생성하는 예약어로 def와 동일하고, 한줄로 간결하게 함수를 만들 때 사용
> def를 쓸 수 없는 상황에 쓰임. map 함수와 자주 쓰인다고 한다

lambda 매개인자1, 매개인자2, ...: 표현식

```python
add = lambda a, b: a+b

print(add(1,2))

print(1,3 )
```

```python
a = [1,2,3,4]
b = [17,12,11,10]
print(list(map(lambda x, y:x+y, a,b)))
#[18, 14, 14, 14]
```
