# [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> where the elements are sorted in <strong>ascending order</strong>, convert <em>it to a </em><span data-keyword="height-balanced"><strong><em>height-balanced</em></strong></span> <em>binary search tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0108.Convert%20Sorted%20Array%20to%20Binary%20Search%20Tree/images/btree1.jpg" style="width: 302px; height: 222px;" />
<pre>
<strong>Input:</strong> nums = [-10,-3,0,5,9]
<strong>Output:</strong> [0,-3,9,-10,null,5]
<strong>Explanation:</strong> [0,-10,5,null,-3,null,9] is also accepted:
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0108.Convert%20Sorted%20Array%20to%20Binary%20Search%20Tree/images/btree2.jpg" style="width: 302px; height: 222px;" />
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0108.Convert%20Sorted%20Array%20to%20Binary%20Search%20Tree/images/btree.jpg" style="width: 342px; height: 142px;" />
<pre>
<strong>Input:</strong> nums = [1,3]
<strong>Output:</strong> [3,1]
<strong>Explanation:</strong> [1,null,3] and [3,1] are both height-balanced BSTs.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>nums</code> is sorted in a <strong>strictly increasing</strong> order.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Search + Recursion

We design a recursive function $dfs(l, r)$, which indicates that the node values of the current binary search tree to be constructed are all within the index range $[l, r]$ of the array `nums`. This function returns the root node of the constructed binary search tree.

The execution process of the function $dfs(l, r)$ is as follows:

1. If $l > r$, it means the current array is empty, return `null`.
2. If $l \leq r$, take the element with the index $mid = \lfloor \frac{l + r}{2} \rfloor$ in the array as the root node of the current binary search tree, where $\lfloor x \rfloor$ represents rounding down $x$.
3. Recursively construct the left subtree of the current binary search tree, whose root node value is the element with the index $mid - 1$ in the array, and the node values of the left subtree are all within the index range $[l, mid - 1]$ of the array.
4. Recursively construct the right subtree of the current binary search tree, whose root node value is the element with the index $mid + 1$ in the array, and the node values of the right subtree are all within the index range $[mid + 1, r]$ of the array.
5. Return the root node of the current binary search tree.

The answer is the return value of the function $dfs(0, n - 1)$.

The time complexity is $O(n)$, and the space complexity is $O(\log n)$. Here, $n$ is the length of the array `nums`.

#### Java

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int[] nums;

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return dfs(0, nums.length - 1);
    }

    private TreeNode dfs(int l, int r) {
        if (l > r) {
            return null;
        }
        int mid = (l + r) >> 1;
        TreeNode left = dfs(l, mid - 1);
        TreeNode right = dfs(mid + 1, r);
        return new TreeNode(nums[mid], left, right);
    }
}
```

#### C++

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        function<TreeNode*(int, int)> dfs = [&](int l, int r) -> TreeNode* {
            if (l > r) {
                return nullptr;
            }
            int mid = (l + r) >> 1;
            auto left = dfs(l, mid - 1);
            auto right = dfs(mid + 1, r);
            return new TreeNode(nums[mid], left, right);
        };
        return dfs(0, nums.size() - 1);
    }
};
```
#### JavaScript

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {number[]} nums
 * @return {TreeNode}
 */
var sortedArrayToBST = function (nums) {
    const dfs = (l, r) => {
        if (l > r) {
            return null;
        }
        const mid = (l + r) >> 1;
        const left = dfs(l, mid - 1);
        const right = dfs(mid + 1, r);
        return new TreeNode(nums[mid], left, right);
    };
    return dfs(0, nums.length - 1);
};
```
<!-- solution:end -->

<!-- problem:end -->