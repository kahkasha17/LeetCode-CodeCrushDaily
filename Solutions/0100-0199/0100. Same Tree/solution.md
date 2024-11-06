# [100. Same Tree](https://leetcode.com/problems/same-tree)

## Description

<!-- description:start -->

<p>Given the roots of two binary trees <code>p</code> and <code>q</code>, write a function to check if they are the same or not.</p>

<p>Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0100.Same%20Tree/images/ex1.jpg" style="width: 622px; height: 182px;" />
<pre>
<strong>Input:</strong> p = [1,2,3], q = [1,2,3]
<strong>Output:</strong> true
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0100.Same%20Tree/images/ex2.jpg" style="width: 382px; height: 182px;" />
<pre>
<strong>Input:</strong> p = [1,2], q = [1,null,2]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Example 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0100.Same%20Tree/images/ex3.jpg" style="width: 622px; height: 182px;" />
<pre>
<strong>Input:</strong> p = [1,2,1], q = [1,1,2]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in both trees is in the range <code>[0, 100]</code>.</li>
	<li><code>-10<sup>4</sup> &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: DFS

We can use the DFS recursive method to solve this problem.

First, determine whether the root nodes of the two binary trees are the same. If both root nodes are null, then the two binary trees are the same. If only one of the root nodes is null, then the two binary trees are definitely different. If both root nodes are not null, then determine whether their values are the same. If they are not the same, then the two binary trees are definitely different. If they are the same, then determine whether the left subtrees of the two binary trees are the same and whether the right subtrees are the same. The two binary trees are the same only when all the above conditions are met.

The time complexity is $O(\min(m, n))$, and the space complexity is $O(\min(m, n))$. Here, $m$ and $n$ are the number of nodes in the two binary trees, respectively. The space complexity mainly depends on the number of layers of recursive calls, which will not exceed the number of nodes in the smaller binary tree.

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == q) return true;
        if (p == null || q == null || p.val != q.val) return false;
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == q) return true;
        if (!p || !q || p->val != q->val) return false;
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
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
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {boolean}
 */
var isSameTree = function (p, q) {
    if (!p && !q) return true;
    if (p && q) {
        return p.val === q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
    return false;
};
```
### Solution 2: BFS

We can also use the BFS iterative method to solve this problem.

First, add the root nodes of the two binary trees to two queues. Each time, take out one node from each of the two queues and perform the following comparison operations. If the values of the two nodes are not the same, then the structures of the two binary trees are definitely different. If the values of the two nodes are the same, then determine whether the child nodes of the two nodes are null. If only the left child node of one node is null, then the structures of the two binary trees are definitely different. If only the right child node of one node is null, then the structures of the two binary trees are definitely different. If the structures of the left and right child nodes are the same, then add the left and right child nodes of the two nodes to the two queues respectively. For the next iteration, take out one node from each of the two queues for comparison. When both queues are empty at the same time, it means that we have compared all the nodes, and the structures of the two binary trees are completely the same.

The time complexity is $O(\min(m, n))$, and the space complexity is $O(\min(m, n))$. Here, $m$ and $n$ are the number of nodes in the two binary trees, respectively. The space complexity mainly depends on the number of elements in the queue, which will not exceed the number of nodes in the smaller binary tree.

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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == q) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        Deque<TreeNode> q1 = new ArrayDeque<>();
        Deque<TreeNode> q2 = new ArrayDeque<>();
        q1.offer(p);
        q2.offer(q);
        while (!q1.isEmpty() && !q2.isEmpty()) {
            p = q1.poll();
            q = q2.poll();
            if (p.val != q.val) {
                return false;
            }
            TreeNode la = p.left, ra = p.right;
            TreeNode lb = q.left, rb = q.right;
            if ((la != null && lb == null) || (lb != null && la == null)) {
                return false;
            }
            if ((ra != null && rb == null) || (rb != null && ra == null)) {
                return false;
            }
            if (la != null) {
                q1.offer(la);
                q2.offer(lb);
            }
            if (ra != null) {
                q1.offer(ra);
                q2.offer(rb);
            }
        }
        return true;
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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == q) return true;
        if (!p || !q) return false;
        queue<TreeNode*> q1{{p}};
        queue<TreeNode*> q2{{q}};
        while (!q1.empty() && !q2.empty()) {
            p = q1.front();
            q = q2.front();
            if (p->val != q->val) return false;
            q1.pop();
            q2.pop();
            TreeNode *la = p->left, *ra = p->right;
            TreeNode *lb = q->left, *rb = q->right;
            if ((la && !lb) || (lb && !la)) return false;
            if ((ra && !rb) || (rb && !ra)) return false;
            if (la) {
                q1.push(la);
                q2.push(lb);
            }
            if (ra) {
                q1.push(ra);
                q2.push(rb);
            }
        }
        return true;
    }
};
```
<!-- solution:end -->

<!-- problem:end -->