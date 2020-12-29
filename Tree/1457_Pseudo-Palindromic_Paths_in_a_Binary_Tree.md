<https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
 - Traverse the tree to get all paths from root to leave
 - Check if each path is a pseudo-palindromic path
 
A pseudo-palindromic path is a path that contains at most 1 odd-frequency number. Here are two ways to check it:
 - Maintain a hash-table or an array to record the frequency for each digit. Once arriving the leaf node, check if there is at most 1 odd-frequency number.
 - Using a 9-bit mask integer <img src="https://render.githubusercontent.com/render/math?math=x"> to record the frequency for each digit. For each bit, 1 means odd frequency and 0 means even frequency.
 For each new node value <img src="https://render.githubusercontent.com/render/math?math=v">, update <img src="https://render.githubusercontent.com/render/math?math=x"> by <img src="http://chart.googleapis.com/chart?cht=tx&chl= x=x \wedge (1 \ll (v - 1))" style="border:none;">
 (flipping the specific bit). Once readching the leaf node, check the pseudo-palindromic property by checking <img src="http://www.sciweavers.org/tex2img.php?eq=x%5C%26%28x-1%29%3D%3D0&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="x\&(x-1)==0" width="129" height="18" />.
```java
class Solution {
    int res = 0;
    public int pseudoPalindromicPaths (TreeNode root) {
        helper(root, 0);
        return res;
    }
    
    public void helper(TreeNode node, int val) {
        if (node != null) {
            val = val ^ (1 << (node.val - 1));
            if (node.left == null && node.right == null) {
                if ((val & (val - 1)) == 0)
                    res ++;
            }
            helper(node.left, val);
            helper(node.right, val);
        }
        
    }
}

// Time complexity: O(num(nodes))
// Memory complexity: O(height(tree))
```
