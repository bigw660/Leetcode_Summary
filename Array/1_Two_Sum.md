<https://leetcode.com/problems/two-sum/> 

## Python:
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        for index, num in enumerate(nums):
            complement = target - num
            if complement in d:
                return [d[complement], index]
            else:
                d[num] = index
        return [-1, -1]
```

## Java:
```java
```
