# [2003. Smallest Missing Genetic Value in Each Subtree](https://leetcode.com/problems/smallest-missing-genetic-value-in-each-subtree)

## Description

<!-- description:start -->

<p>There is a <strong>family tree</strong> rooted at <code>0</code> consisting of <code>n</code> nodes numbered <code>0</code> to <code>n - 1</code>. You are given a <strong>0-indexed</strong> integer array <code>parents</code>, where <code>parents[i]</code> is the parent for node <code>i</code>. Since node <code>0</code> is the <strong>root</strong>, <code>parents[0] == -1</code>.</p>

<p>There are <code>10<sup>5</sup></code> genetic values, each represented by an integer in the <strong>inclusive</strong> range <code>[1, 10<sup>5</sup>]</code>. You are given a <strong>0-indexed</strong> integer array <code>nums</code>, where <code>nums[i]</code> is a <strong>distinct </strong>genetic value for node <code>i</code>.</p>

<p>Return <em>an array </em><code>ans</code><em> of length </em><code>n</code><em> where </em><code>ans[i]</code><em> is</em> <em>the <strong>smallest</strong> genetic value that is <strong>missing</strong> from the subtree rooted at node</em> <code>i</code>.</p>

<p>The <strong>subtree</strong> rooted at a node <code>x</code> contains node <code>x</code> and all of its <strong>descendant</strong> nodes.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2003.Smallest%20Missing%20Genetic%20Value%20in%20Each%20Subtree/images/case-1.png" style="width: 204px; height: 167px;" />
<pre>
<strong>Input:</strong> parents = [-1,0,0,2], nums = [1,2,3,4]
<strong>Output:</strong> [5,1,1,1]
<strong>Explanation:</strong> The answer for each subtree is calculated as follows:
- 0: The subtree contains nodes [0,1,2,3] with values [1,2,3,4]. 5 is the smallest missing value.
- 1: The subtree contains only node 1 with value 2. 1 is the smallest missing value.
- 2: The subtree contains nodes [2,3] with values [3,4]. 1 is the smallest missing value.
- 3: The subtree contains only node 3 with value 4. 1 is the smallest missing value.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2003.Smallest%20Missing%20Genetic%20Value%20in%20Each%20Subtree/images/case-2.png" style="width: 247px; height: 168px;" />
<pre>
<strong>Input:</strong> parents = [-1,0,1,0,3,3], nums = [5,4,6,2,1,3]
<strong>Output:</strong> [7,1,1,4,2,1]
<strong>Explanation:</strong> The answer for each subtree is calculated as follows:
- 0: The subtree contains nodes [0,1,2,3,4,5] with values [5,4,6,2,1,3]. 7 is the smallest missing value.
- 1: The subtree contains nodes [1,2] with values [4,6]. 1 is the smallest missing value.
- 2: The subtree contains only node 2 with value 6. 1 is the smallest missing value.
- 3: The subtree contains nodes [3,4,5] with values [2,1,3]. 4 is the smallest missing value.
- 4: The subtree contains only node 4 with value 1. 2 is the smallest missing value.
- 5: The subtree contains only node 5 with value 3. 1 is the smallest missing value.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> parents = [-1,2,3,0,2,4,1], nums = [2,3,4,5,6,7,8]
<strong>Output:</strong> [1,1,1,1,1,1,1]
<strong>Explanation:</strong> The value 1 is missing from all the subtrees.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == parents.length == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= parents[i] &lt;= n - 1</code> for <code>i != 0</code></li>
	<li><code>parents[0] == -1</code></li>
	<li><code>parents</code> represents a valid tree.</li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Each <code>nums[i]</code> is distinct.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We notice that each node has a unique gene value, so we only need to find the node $idx$ with gene value $1$, and all nodes except for those on the path from node $idx$ to the root node $0$ have an answer of $1$.

Therefore, we initialize the answer array $ans$ to $[1,1,...,1]$, and our focus is on finding the answer for each node on the path from node $idx$ to the root node $0$.

We can start from node $idx$ and use depth-first search to mark the gene values that appear in the subtree rooted at $idx$, and record them in the array $has$. During the search process, we use an array $vis$ to mark the visited nodes to prevent repeated visits.

Next, we start from $i=2$ and keep looking for the first gene value that has not appeared, which is the answer for node $idx$. Here, $i$ is strictly increasing, because the gene values are unique, so we can always find a gene value that has not appeared in $[1,..n+1]$.

Then, we update the answer for node $idx$, i.e., $ans[idx]=i$, and update $idx$ to its parent node to continue the above process until $idx=-1$, which means we have reached the root node $0$.

Finally, we return the answer array $ans$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the number of nodes.

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private boolean[] vis;
    private boolean[] has;
    private int[] nums;

    public int[] smallestMissingValueSubtree(int[] parents, int[] nums) {
        int n = nums.length;
        this.nums = nums;
        g = new List[n];
        vis = new boolean[n];
        has = new boolean[n + 2];
        Arrays.setAll(g, i -> new ArrayList<>());
        int idx = -1;
        for (int i = 0; i < n; ++i) {
            if (i > 0) {
                g[parents[i]].add(i);
            }
            if (nums[i] == 1) {
                idx = i;
            }
        }
        int[] ans = new int[n];
        Arrays.fill(ans, 1);
        if (idx == -1) {
            return ans;
        }
        for (int i = 2; idx != -1; idx = parents[idx]) {
            dfs(idx);
            while (has[i]) {
                ++i;
            }
            ans[idx] = i;
        }
        return ans;
    }

    private void dfs(int i) {
        if (vis[i]) {
            return;
        }
        vis[i] = true;
        if (nums[i] < has.length) {
            has[nums[i]] = true;
        }
        for (int j : g[i]) {
            dfs(j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestMissingValueSubtree(vector<int>& parents, vector<int>& nums) {
        int n = nums.size();
        vector<int> g[n];
        bool vis[n];
        bool has[n + 2];
        memset(vis, false, sizeof(vis));
        memset(has, false, sizeof(has));
        int idx = -1;
        for (int i = 0; i < n; ++i) {
            if (i) {
                g[parents[i]].push_back(i);
            }
            if (nums[i] == 1) {
                idx = i;
            }
        }
        vector<int> ans(n, 1);
        if (idx == -1) {
            return ans;
        }
        function<void(int)> dfs = [&](int i) {
            if (vis[i]) {
                return;
            }
            vis[i] = true;
            if (nums[i] < n + 2) {
                has[nums[i]] = true;
            }
            for (int j : g[i]) {
                dfs(j);
            }
        };
        for (int i = 2; ~idx; idx = parents[idx]) {
            dfs(idx);
            while (has[i]) {
                ++i;
            }
            ans[idx] = i;
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->