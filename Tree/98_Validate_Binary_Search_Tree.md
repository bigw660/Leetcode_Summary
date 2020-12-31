<https://leetcode.com/problems/validate-binary-search-tree/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:

```java
class Solution {
    Integer prev;
    public boolean isValidBST(TreeNode root) {
        return helper(root);
    }
    
    public boolean helper(TreeNode node) {
        if (node == null)
            return true;
        if (!helper(node.left))
            return false;
        if (prev != null && prev >= node.val) {
            return false;
        }
        prev = node.val;
        return helper(node.right);
    }
}

// Time complexity: O(n)
// Memory complexity: O(n)
```
