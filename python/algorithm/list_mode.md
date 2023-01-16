# list 최빈값 구하기

### 내장함수인 max함수와 key 인자 사용

</br>

```python
nums = [1, 1, 1, 3, 4, 2, 2, 2, 2]
mode = max(nums, key = nums.count)
# or
mode = max(nums, key = lambda x : nums.count(x))
```
