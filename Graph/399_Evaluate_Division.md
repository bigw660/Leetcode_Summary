<https://leetcode.com/problems/evaluate-division/> 

## Python:
```python

        
# Time complexity: 
# Space complexity: 
```

## Java:
### Thoughts:
Key thought: Build a weighted union-find set to store the relations between different nodes. In detail, keep the weight for each node as the ratio of its root node. Then,
during querying stages, first check if two nodes have the same root. If yes, divide their weights to get solution; otherwise, return -1.0. The biggest challenge here is to
update the weight of each node when its root node is changed. Path compression is a good solution for this problem.

The path compression is applied in find function. When the find function is called, it would redirect all the nodes along the searching path to the root node and update their
weights recursively. The psudo code and demo for this are shown as follows:
<img src="https://github.com/bigw660/Leetcode_Summary/blob/main/Images/path_compression.jpg"/>

Since both the union and query function will call find function first, so there is at most 1 step away from each node to its root, which means every two connected nodes share
the same root.
```java
class Solution {
    class UnionFind { // definition of the union-find set class
        int[] parent;
        double[] weight;

        public UnionFind(int n) {
            parent = new int[n];
            weight = new double[n];
            for (int i = 0; i < n; ++i) {
                parent[i] = i;
                weight[i] = 1.0;
            }
        }

        public void union(int i, int j, double val) {
            int rootI = find(i);
            int rootJ = find(j);
            if (rootI != rootJ) {
                parent[rootI] = rootJ;
                weight[rootI] = val * weight[j] / weight[i];
            }
        }

        public int find(int i) {
            // path compression makes at most 1 step from root to node
            if (i != parent[i]) {
                int p = parent[i];
                parent[i] = find(parent[i]);
                weight[i] *= weight[p];
            }
            return parent[i];
        }

        public double query(int i, int j) {
            if (find(i) != find(j)) {
                return -1.0;
            }
            return weight[i] / weight[j]; // if two nodes are connected, they share the same root and their weights corresponds to such root.
        }
    }
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        int n = equations.size();
        UnionFind uf = new UnionFind(2 * n);
        Map<String, Integer> map = new HashMap<>();
        int id = 0;
        for (int i = 0; i < n; ++i) {
            List<String> eq = equations.get(i);
            double val = values[i];
            
            // map each string to a unique id, help build the union-find set
            if (!map.containsKey(eq.get(0))) {
                map.put(eq.get(0), id++);
            }
            if (!map.containsKey(eq.get(1))) {
                map.put(eq.get(1), id++);
            }

            uf.union(map.get(eq.get(0)), map.get(eq.get(1)), val);
        }

        double[] res = new double[queries.size()];
        for (int i = 0; i < queries.size(); ++i) {
            List<String> qr = queries.get(i);
            if (!map.containsKey(qr.get(0)) || !map.containsKey(qr.get(1))) { // if the query string is not included by the equations, return -1.0
                res[i] = -1.0;
            }
            else {
                res[i] = uf.query(map.get(qr.get(0)), map.get(qr.get(1)));
            }
        }

        return res;
    }
}

// Time complexity: O((N+M) log A). O(N log A) for building union-find set, O(M log A) for queries. N: size of equations; M: size of queries; A: size of union-find set.
// Memory complexity: O(A)
```
