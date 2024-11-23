# [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

## Description

<!-- description:start -->

<p>Given the <code>root</code> of a binary search tree, and an integer <code>k</code>, return <em>the</em> <code>k<sup>th</sup></code> <em>smallest value (<strong>1-indexed</strong>) of all the values of the nodes in the tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0230.Kth%20Smallest%20Element%20in%20a%20BST/images/kthtree1.jpg" style="width: 212px; height: 301px;" />
<pre>
<strong>Input:</strong> root = [3,1,4,null,2], k = 1
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0230.Kth%20Smallest%20Element%20in%20a%20BST/images/kthtree2.jpg" style="width: 382px; height: 302px;" />
<pre>
<strong>Input:</strong> root = [5,3,6,2,4,null,null,1], k = 3
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is <code>n</code>.</li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        stk = []
        while root or stk:
            if root:
                stk.append(root)
                root = root.left
            else:
                root = stk.pop()
                k -= 1
                if k == 0:
                    return root.val
                root = root.right
```

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
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stk = new ArrayDeque<>();
        while (root != null || !stk.isEmpty()) {
            if (root != null) {
                stk.push(root);
                root = root.left;
            } else {
                root = stk.pop();
                if (--k == 0) {
                    return root.val;
                }
                root = root.right;
            }
        }
        return 0;
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
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*> stk;
        while (root || !stk.empty()) {
            if (root) {
                stk.push(root);
                root = root->left;
            } else {
                root = stk.top();
                stk.pop();
                if (--k == 0) return root->val;
                root = root->right;
            }
        }
        return 0;
    }
};
```
### Solution 2

#### Python3

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right


class BST:
    def __init__(self, root):
        self.cnt = Counter()
        self.root = root
        self.count(root)

    def kthSmallest(self, k):
        node = self.root
        while node:
            if self.cnt[node.left] == k - 1:
                return node.val
            if self.cnt[node.left] < k - 1:
                k -= self.cnt[node.left] + 1
                node = node.right
            else:
                node = node.left
        return 0

    def count(self, root):
        if root is None:
            return 0
        n = 1 + self.count(root.left) + self.count(root.right)
        self.cnt[root] = n
        return n


class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        bst = BST(root)
        return bst.kthSmallest(k)
```

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
    public int kthSmallest(TreeNode root, int k) {
        BST bst = new BST(root);
        return bst.kthSmallest(k);
    }
}

class BST {
    private TreeNode root;
    private Map<TreeNode, Integer> cnt = new HashMap<>();

    public BST(TreeNode root) {
        this.root = root;
        count(root);
    }

    public int kthSmallest(int k) {
        TreeNode node = root;
        while (node != null) {
            int v = node.left == null ? 0 : cnt.get(node.left);
            if (v == k - 1) {
                return node.val;
            }
            if (v < k - 1) {
                node = node.right;
                k -= (v + 1);
            } else {
                node = node.left;
            }
        }
        return 0;
    }

    private int count(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int n = 1 + count(root.left) + count(root.right);
        cnt.put(root, n);
        return n;
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
class BST {
public:
    BST(TreeNode* root)
        : root(root) {
        count(root);
    }

    int kthSmallest(int k) {
        TreeNode* node = root;
        while (node) {
            int v = !node->left ? 0 : cnt[node->left];
            if (v == k - 1) return node->val;
            if (v < k - 1) {
                node = node->right;
                k -= (v + 1);
            } else
                node = node->left;
        }
        return 0;
    }

private:
    TreeNode* root;
    unordered_map<TreeNode*, int> cnt;

    int count(TreeNode* root) {
        if (!root) return 0;
        int n = 1 + count(root->left) + count(root->right);
        cnt[root] = n;
        return n;
    }
};

class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        BST bst(root);
        return bst.kthSmallest(k);
    }
};
```

<!-- solution:end -->

<!-- problem:end -->