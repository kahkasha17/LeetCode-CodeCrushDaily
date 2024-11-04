# [78. Subsets](https://leetcode.com/problems/subsets)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> of <strong>unique</strong> elements, return <em>all possible</em> <span data-keyword="subset"><em>subsets</em></span> <em>(the power set)</em>.</p>

<p>The solution set <strong>must not</strong> contain duplicate subsets. Return the solution in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [0]
<strong>Output:</strong> [[],[0]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
	<li>All the numbers of&nbsp;<code>nums</code> are <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS (Backtracking)

We design a function $dfs(i)$, which represents starting the search from the $i$th element of the array for all subsets. The execution logic of the function $dfs(i)$ is as follows:

-   If $i = n$, it means the current search has ended. Add the current subset $t$ to the answer array $ans$, and then return.
-   Otherwise, we can choose not to select the current element and directly execute $dfs(i + 1)$; or we can choose the current element, i.e., add the current element $nums[i]$ to the subset $t$, and then execute $dfs(i + 1)$. Note that we need to remove $nums[i]$ from the subset $t$ after executing $dfs(i + 1)$ (backtracking).

In the main function, we call $dfs(0)$, i.e., start searching all subsets from the first element of the array. Finally, return the answer array $ans$.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array. There are a total of $2^n$ subsets, and each subset takes $O(n)$ time to construct.

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int[] nums;

    public List<List<Integer>> subsets(int[] nums) {
        this.nums = nums;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i == nums.length) {
            ans.add(new ArrayList<>(t));
            return;
        }
        dfs(i + 1);
        t.add(nums[i]);
        dfs(i + 1);
        t.remove(t.size() - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> t;
        function<void(int)> dfs = [&](int i) -> void {
            if (i == nums.size()) {
                ans.push_back(t);
                return;
            }
            dfs(i + 1);
            t.push_back(nums[i]);
            dfs(i + 1);
            t.pop_back();
        };
        dfs(0);
        return ans;
    }
};
```

### Solution 2: Binary Enumeration

We can also use the method of binary enumeration to get all subsets.

We can use $2^n$ binary numbers to represent all subsets of $n$ elements. For the current binary number $mask$, if the $i$th bit is $1$, it means that the $i$th element is selected, otherwise it means that the $i$th element is not selected.

The time complexity is $O(n \times 2^n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array. There are a total of $2^n$ subsets, and each subset takes $O(n)$ time to construct.

#### Java

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int mask = 0; mask < 1 << n; ++mask) {
            List<Integer> t = new ArrayList<>();
            for (int i = 0; i < n; ++i) {
                if (((mask >> i) & 1) == 1) {
                    t.add(nums[i]);
                }
            }
            ans.add(t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> ans;
        for (int mask = 0; mask < 1 << n; ++mask) {
            vector<int> t;
            for (int i = 0; i < n; ++i) {
                if (mask >> i & 1) {
                    t.emplace_back(nums[i]);
                }
            }
            ans.emplace_back(t);
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->