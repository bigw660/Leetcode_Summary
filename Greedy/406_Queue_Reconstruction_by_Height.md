<https://leetcode.com/problems/queue-reconstruction-by-height/>

## Python
```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        # sort the height in descending order
        # if two people has the same height, sort k in asceding order
        people.sort(key = lambda x: (-x[0], x[1]))
        result = []
        for p in people:
            # By sorting the array of people based on height (tallest first)
            # it is guaranteed that if you put the person on index based on 
            # the p[1] that it will have p[1] many taller people before him.
            result.insert(p[1], p)
        return result
        
# Time complexity: O(N^2) -> similar to insertion sort
# Space complexity: O(Height)
```

## Java
