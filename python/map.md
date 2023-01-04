# map 함수

> 사용 형식 map(function, iterable)
> 자료형을 튜플 형식으로 묶어서 반환해준다.

```python
# 상황1. 리스트에 담긴 float 요소들을 int로 바꾸기
a = [1.2, 2.5, 3.7, 4.6]
for i in range(len(a)):
    a[i] = int(a[i])
# 위와 같이 할 수도 있지만

a = [1.2, 2.5, 3.7, 4.6]

a = list(map(int, a))
# map 함수를 사용하면 더 간결한 코드가 완성!
```

```python
# 알고리즘에 자주 쓰이는 숫자 두개 입력 받기

a,b = map(int, input('정수 두개를 입력하시오.').split())

```
