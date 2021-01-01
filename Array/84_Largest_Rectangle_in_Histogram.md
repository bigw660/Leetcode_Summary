<https://leetcode-cn.com/problems/largest-rectangle-in-histogram/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
For each bar b_i, find the left-most bar l_i and right-most bar r_i where the bars in range [l_i, r_i] are all higher than or equal to b_i. Thus, for bar_i, the largeset area should be
(r_i - l_i + 1) * b_i. Repeat the calculation for each bar and maintain the maximum value as the result. So the key problem becomes how to quickly find the left-most and right-most bars.

We can use the monotonic stack to solve this problem. Here the monotonic increasing stack is employed. 
- First, traverse the array from left to right and build the monotonic increasing stack on the fly. So for each bar b_i, the top element of the stack should be the first left element
that is smaller than b_i (or -1 if the stack is empty). Update the l_i for each bar with this assumption.
- Repeat the algorithm once again from right to left to calculate the r_i for each bar.
- Calculate the area for each bar and maintain the biggest one.

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int[] left = new int[n], right = new int[n];

        Stack<Integer> st = new Stack<>();

        for (int i = 0; i < n; ++i) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }
            left[i] = st.isEmpty()? -1 : st.peek();
            st.push(i);
        }

        st.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!st.isEmpty() && heights[st.peek()] >= heights[i]) {
                st.pop();
            }
            right[i] = st.isEmpty()? n : st.peek();
            st.push(i);
        }

        int res = 0;
        for (int i = 0; i < n; ++i) {
            res = Math.max(res, (right[i] - left[i] - 1) * heights[i]);
        }

        return res;
    }
}
// Time complexity: O(N)
// Memory complexity: O(N)
```
