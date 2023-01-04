# enumerate 함수

> 반복문을 사용할때 인덱스 번호와 원소를 tuple 형태로 반환해주는 용도로 자주 사용한다.

```python
a = [5, 4, 3, 2, 1]

for i in enumerate(a):
    print(i)

''' 출력
(0, 5)
(1, 4)
(2, 3)
(3, 2)
(4, 1)
'''

혹은

for i, j in enumerate(a):
    print(i, j)

'''출력
0 5
1 4
2 3
3 2
4 1
'''
```
