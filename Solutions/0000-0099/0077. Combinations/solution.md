# [77. Combinations](https://leetcode.com/problems/combinations)

## Description

<!-- description:start -->

<p>Given two integers <code>n</code> and <code>k</code>, return <em>all possible combinations of</em> <code>k</code> <em>numbers chosen from the range</em> <code>[1, n]</code>.</p>

<p>You may return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 4, k = 2
<strong>Output:</strong> [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
<strong>Explanation:</strong> There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 1, k = 1
<strong>Output:</strong> [[1]]
<strong>Explanation:</strong> There is 1 choose 1 = 1 total combination.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 20</code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Backtracking (Two Ways)

We design a function $dfs(i)$, which represents starting the search from number $i$, with the current search path as $t$, and the answer as $ans$.

The execution logic of the function $dfs(i)$ is as follows:

-   If the length of the current search path $t$ equals $k$, then add the current search path to the answer and return.
-   If $i \gt n$, it means the search has ended, return.
-   Otherwise, we can choose to add the number $i$ to the search path $t$, and then continue the search, i.e., execute $dfs(i + 1)$, and then remove the number $i$ from the search path $t$; or we do not add the number $i$ to the search path $t$, and directly execute $dfs(i + 1)$.

The above method is actually enumerating whether to select the current number or not, and then recursively searching the next number. We can also enumerate the next number $j$ to be selected, where $i \leq j \leq n$. If the next number to be selected is $j$, then we add the number $j$ to the search path $t$, and then continue the search, i.e., execute $dfs(j + 1)$, and then remove the number $j$ from the search path $t$.

In the main function, we start the search from number $1$, i.e., execute $dfs(1)$.

The time complexity is $(C_n^k \times k)$, and the space complexity is $O(k)$. Here, $C_n^k$ represents the combination number.

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int n;
    private int k;

    public List<List<Integer>> combine(int n, int k) {
        this.n = n;
        this.k = k;
        dfs(1);
        return ans;
    }

    private void dfs(int i) {
        if (t.size() == k) {
            ans.add(new ArrayList<>(t));
            return;
        }
        if (i > n) {
            return;
        }
        t.add(i);
        dfs(i + 1);
        t.remove(t.size() - 1);
        dfs(i + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> t;
        function<void(int)> dfs = [&](int i) {
            if (t.size() == k) {
                ans.emplace_back(t);
                return;
            }
            if (i > n) {
                return;
            }
            t.emplace_back(i);
            dfs(i + 1);
            t.pop_back();
            dfs(i + 1);
        };
        dfs(1);
        return ans;
    }
};
```

### Solution 2
#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int n;
    private int k;

    public List<List<Integer>> combine(int n, int k) {
        this.n = n;
        this.k = k;
        dfs(1);
        return ans;
    }

    private void dfs(int i) {
        if (t.size() == k) {
            ans.add(new ArrayList<>(t));
            return;
        }
        if (i > n) {
            return;
        }
        for (int j = i; j <= n; ++j) {
            t.add(j);
            dfs(j + 1);
            t.remove(t.size() - 1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> t;
        function<void(int)> dfs = [&](int i) {
            if (t.size() == k) {
                ans.emplace_back(t);
                return;
            }
            if (i > n) {
                return;
            }
            for (int j = i; j <= n; ++j) {
                t.emplace_back(j);
                dfs(j + 1);
                t.pop_back();
            }
        };
        dfs(1);
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->