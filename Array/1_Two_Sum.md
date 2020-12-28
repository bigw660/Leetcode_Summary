<https://leetcode.com/problems/two-sum/> \

## Python Code:
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        for i, v in enumerate(nums):
            if target - v in d:
                return [d[target - v], i]
            else:
                d[v] = i
        return [-1, -1]
```
