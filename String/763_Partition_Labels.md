<https://leetcode.com/problems/partition-labels/>
## Python
```python
class Solution:
    def partitionLabels(self, S: str) -> List[int]:
        d = {char : index for index, char in enumerate(S)} # {num : last index of the num}
        right_most_index = 0
        start_index = 0
        result = []
        for index, char in enumerate(S):
            # update the end index of the current substring 
            right_most_index = max(right_most_index, d[char])
            if index == right_most_index:
                # the length of substring needs to consider the starting index
                result.append(index - start_index + 1)
                # update the starting index
                start_index = index + 1
        return result
        
# Time complexity: O(N)
# Space complexity: O(N)
```

## Java
