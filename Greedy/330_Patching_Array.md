<https://leetcode-cn.com/problems/patching-array/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
Assume the range <img src="https://render.githubusercontent.com/render/math?math=\[1, n)"> is already covered by some elements of the array. Consider two cases:
  - If no element <img src="https://render.githubusercontent.com/render/math?math=x"> in the rest numbers satisfies <img src="https://render.githubusercontent.com/render/math?math=x \leq n">,
then the value <img src="https://render.githubusercontent.com/render/math?math=n"> can never be covered. Thus, it needs to add new element <img src="https://render.githubusercontent.com/render/math?math=n">. After adding element <img src="https://render.githubusercontent.com/render/math?math=n">,
the new range that is covered could be <img src="https://render.githubusercontent.com/render/math?math=\[1, 2n)"> 
  - If the element <img src="https://render.githubusercontent.com/render/math?math=x"> in the rest numbers satisfies <img src="https://render.githubusercontent.com/render/math?math=x \leq n">,
then the new range that is covered could be <img src="https://render.githubusercontent.com/render/math?math=\[1, x %2B n)">.

Since the array is already sorted, just scan it from left to right. Initialize the covered range as <img src="https://render.githubusercontent.com/render/math?math=\[1, 1)"> (trivally valid). Repeating the above-mentioned step for each element to update the range until the right side of the range is larger than the target.
```java
class Solution {
    public int minPatches(int[] nums, int n) {
        int pos = 0;
        long rightSide = 1; // [1, rightSide) is covered
        int res = 0;
        while (rightSide <= (long)n) {
            if (pos < nums.length && nums[pos] <= rightSide) {
                rightSide += nums[pos ++];
            }
            else {
                res ++;
                rightSide *= 2;
            }
        }
        return res;
    }
}

// Time complexity: O(size(nums) + logn)
// Memory complexity: O(1)
```
