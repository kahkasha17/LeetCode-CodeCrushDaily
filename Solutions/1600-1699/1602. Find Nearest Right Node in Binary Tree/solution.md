
# [1602. Find Nearest Right Node in Binary Tree 🔒](https://leetcode.com/problems/find-nearest-right-node-in-binary-tree)


## Description

<!-- description:start -->

<p>Given the <code>root</code> of a binary tree and a node <code>u</code> in the tree, return <em>the <strong>nearest</strong> node on the <strong>same level</strong> that is to the <strong>right</strong> of</em> <code>u</code><em>, or return</em> <code>null</code> <em>if </em><code>u</code> <em>is the rightmost node in its level</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1602.Find%20Nearest%20Right%20Node%20in%20Binary%20Tree/images/p3.png" style="width: 241px; height: 161px;" />
<pre>
<strong>Input:</strong> root = [1,2,3,null,4,5,6], u = 4
<strong>Output:</strong> 5
<strong>Explanation:</strong> The nearest node on the same level to the right of node 4 is node 5.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1600-1699/1602.Find%20Nearest%20Right%20Node%20in%20Binary%20Tree/images/p2.png" style="width: 101px; height: 161px;" />
<pre>
<strong>Input:</strong> root = [3,null,4,2], u = 2
<strong>Output:</strong> null
<strong>Explanation:</strong> There are no nodes to the right of 2.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li>All values in the tree are <strong>distinct</strong>.</li>
	<li><code>u</code> is a node in the binary tree rooted at <code>root</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: BFS

We can use Breadth-First Search, starting from the root node. When we reach node $u$, we return the next node in the queue.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the number of nodes in the binary tree.


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
    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        Deque<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            for (int i = q.size(); i > 0; --i) {
                root = q.pollFirst();
                if (root == u) {
                    return i > 1 ? q.peekFirst() : null;
                }
                if (root.left != null) {
                    q.offer(root.left);
                }
                if (root.right != null) {
                    q.offer(root.right);
                }
            }
        }
        return null;
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
    TreeNode* findNearestRightNode(TreeNode* root, TreeNode* u) {
        queue<TreeNode*> q{{root}};
        while (q.size()) {
            for (int i = q.size(); i; --i) {
                root = q.front();
                q.pop();
                if (root == u) {
                    return i > 1 ? q.front() : nullptr;
                }
                if (root->left) {
                    q.push(root->left);
                }
                if (root->right) {
                    q.push(root->right);
                }
            }
        }
        return nullptr;
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
 * @param {TreeNode} root
 * @param {TreeNode} u
 * @return {TreeNode}
 */
var findNearestRightNode = function (root, u) {
    const q = [root];
    while (q.length) {
        for (let i = q.length; i; --i) {
            root = q.shift();
            if (root == u) {
                return i > 1 ? q[0] : null;
            }
            if (root.left) {
                q.push(root.left);
            }
            if (root.right) {
                q.push(root.right);
            }
        }
    }
    return null;
};
```


<!-- solution:end -->

<!-- solution:start -->

### Solution 2: DFS

DFS performs a pre-order traversal of the binary tree. The first time we search to node $u$, we mark the current depth $d$. The next time we encounter a node at the same level, it is the target node.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the number of nodes in the binary tree.


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
    private TreeNode u;
    private TreeNode ans;
    private int d;

    public TreeNode findNearestRightNode(TreeNode root, TreeNode u) {
        this.u = u;
        dfs(root, 1);
        return ans;
    }

    private void dfs(TreeNode root, int i) {
        if (root == null || ans != null) {
            return;
        }
        if (d == i) {
            ans = root;
            return;
        }
        if (root == u) {
            d = i;
            return;
        }
        dfs(root.left, i + 1);
        dfs(root.right, i + 1);
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
    TreeNode* findNearestRightNode(TreeNode* root, TreeNode* u) {
        TreeNode* ans;
        int d = 0;
        function<void(TreeNode*, int)> dfs = [&](TreeNode* root, int i) {
            if (!root || ans) {
                return;
            }
            if (d == i) {
                ans = root;
                return;
            }
            if (root == u) {
                d = i;
                return;
            }
            dfs(root->left, i + 1);
            dfs(root->right, i + 1);
        };
        dfs(root, 1);
        return ans;
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
 * @param {TreeNode} root
 * @param {TreeNode} u
 * @return {TreeNode}
 */
var findNearestRightNode = function (root, u) {
    let d = 0;
    let ans = null;
    function dfs(root, i) {
        if (!root || ans) {
            return;
        }
        if (d == i) {
            ans = root;
            return;
        }
        if (root == u) {
            d = i;
            return;
        }
        dfs(root.left, i + 1);
        dfs(root.right, i + 1);
    }
    dfs(root, 1);
    return ans;
};
```

<!-- solution:end -->

<!-- problem:end -->