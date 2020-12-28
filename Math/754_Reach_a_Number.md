<https://leetcode.com/problems/reach-a-number/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
Think of the solution to reach the target. It should be a cumulated sum from 1 to n but with some minus operations. When flipping the operation from add to minus for an integer <img src="http://chart.googleapis.com/chart?cht=tx&chl= i" style="border:none;">, 
the sum would decrement by <img src="http://chart.googleapis.com/chart?cht=tx&chl= 2*i" style="border:none;"> which is a even number. So the solution should be the minimal n where <img src="http://chart.googleapis.com/chart?cht=tx&chl= \sum_{i=1}^{n}i - target" style="border:none;"> is a even number.
```java
class Solution {
    public int reachNumber(int target) {
        if (target < 0)
            target = 0 - target; // if target is negative, turn it into positive
        int i = 1, sum = 1;
        while (sum < target || (sum - target) % 2 != 0) {
            sum += (++i);
        }
        
        return i;
    }
}

// Time complexity: O(sqrt(target))
// Memory complexity: O(1)
```
