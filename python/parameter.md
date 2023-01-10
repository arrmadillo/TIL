# 함수 파라미터

## `고정파라미터`

```python
def print_anything(a, b):
    return a, b

print_anything(3, 5)
# (3, 5)
print_anything(3)
# 에러!
```

- #### 고정 파라미터 함수 호출시 파라미터 개수에 맞게 인자를 넘겨야 한다.
- #### 여러 변수를 return 할땐 튜플 형식으로 반환된다.

## `기본 매개변수`

```python
def print_anything(a, b=5):
    return a, b

print_anything(3, 1)
# (3, 1)
print_anything(3)
# (3, 5) b=5 기본값으로 출력
```

#### 파라미터의 기본값이 설정되어 있으면, 일부 인자로도 호출이 가능하다.

<br/>

## `절대로 기본매개변수를 맨 앞에 놓지마😡`

```python
def print_anything(a = 5, b):
    return a, b

print_anything(3, 1)
# (3, 1)
print_anything(3)
# SyntaxError: non-default argument follows default argument
```

#### 왜 오류가 나는 것일까?

- 위 코드에서 오류가 나는 이유는 사용자가 전달한 인자값이 a와 b중 어떤 값인지(어떤 의도인지) 확실하지 않기 때문이다.

## `가변인자 (*args)`

```python
def print_anything(*nums):
        return nums

print(print_anything(3, 1, 4, 2))
# (3, 1, 4, 2)
```

- #### 매개변수를 지정하지 않고, 넘긴 매개변수를 모두 튜플로 인식하여 동작을 수행한다.
- #### 매개변수 앞에 \*를 붙여 인식한다.

## `가변 키워드 인자 (**kargs)`

```python
def print_anything(**kwargs):
        print(kwargs)

print_anything(name='kim', age = 20)
# {'name': 'kim', 'age': 20}
```

- #### 가변인자(\*args)와 유사하게 매개변수를 지정하지 않고 받은 인자를 딕셔너리로 인식한다.
- #### 매개변수 앞에 \*\*를 붙여 인식한다.
