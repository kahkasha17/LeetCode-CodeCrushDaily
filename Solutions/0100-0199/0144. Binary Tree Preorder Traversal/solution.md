# [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal)

## Description

<!-- description:start -->

<p>Given the <code>root</code> of a binary tree, return <em>the preorder traversal of its nodes&#39; values</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">root = [1,null,2,3]</span></p>

<p><strong>Output:</strong> <span class="example-io">[1,2,3]</span></p>

<p><strong>Explanation:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0144.Binary%20Tree%20Preorder%20Traversal/images/screenshot-2024-08-29-202743.png" style="width: 200px; height: 264px;" /></p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">root = [1,2,3,4,5,null,8,null,null,6,7,9]</span></p>

<p><strong>Output:</strong> <span class="example-io">[1,2,4,5,6,7,3,8,9]</span></p>

<p><strong>Explanation:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0144.Binary%20Tree%20Preorder%20Traversal/images/tree_2.png" style="width: 350px; height: 286px;" /></p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">root = []</span></p>

<p><strong>Output:</strong> <span class="example-io">[]</span></p>
</div>

<p><strong class="example">Example 4:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">root = [1]</span></p>

<p><strong>Output:</strong> <span class="example-io">[1]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Recursive solution is trivial, could you do it iteratively?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Recursive Traversal

We first visit the root node, then recursively traverse the left and right subtrees.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the number of nodes in the binary tree. The space complexity mainly depends on the stack space used for recursive calls.

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
    private List<Integer> ans = new ArrayList<>();

    public List<Integer> preorderTraversal(TreeNode root) {
        dfs(root);
        return ans;
    }

    private void dfs(TreeNode root) {
        if (root == null) {
            return;
        }
        ans.add(root.val);
        dfs(root.left);
        dfs(root.right);
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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        function<void(TreeNode*)> dfs = [&](TreeNode* root) {
            if (!root) {
                return;
            }
            ans.push_back(root->val);
            dfs(root->left);
            dfs(root->right);
        };
        dfs(root);
        return ans;
    }
};
```
### Solution 2: Stack Implementation for Non-Recursive Traversal

The idea of using a stack to implement non-recursive traversal is as follows:

1. Define a stack $stk$, and first push the root node into the stack.
2. If the stack is not empty, pop a node from the stack each time.
3. Process the node.
4. First push the right child of the node into the stack, then push the left child of the node into the stack (if there are child nodes).
5. Repeat steps 2-4.
6. Return the result.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the number of nodes in the binary tree. The space complexity mainly depends on the stack space.

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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) {
            return ans;
        }
        Deque<TreeNode> stk = new ArrayDeque<>();
        stk.push(root);
        while (!stk.isEmpty()) {
            TreeNode node = stk.pop();
            ans.add(node.val);
            if (node.right != null) {
                stk.push(node.right);
            }
            if (node.left != null) {
                stk.push(node.left);
            }
        }
        return ans;
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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (!root) {
            return ans;
        }
        stack<TreeNode*> stk;
        stk.push(root);
        while (stk.size()) {
            auto node = stk.top();
            stk.pop();
            ans.push_back(node->val);
            if (node->right) {
                stk.push(node->right);
            }
            if (node->left) {
                stk.push(node->left);
            }
        }
        return ans;
    }
};
```
### Solution 3: Morris Preorder Traversal

Morris traversal does not require a stack, and its space complexity is $O(1)$. The core idea is:

Traverse the binary tree nodes,

1. If the left subtree of the current node `root` is empty, add the current node value to the result list $ans$, and update the current node to `root.right`.
1. If the left subtree of the current node `root` is not empty, find the rightmost node `pre` of the left subtree (which is the predecessor of the `root` node in inorder traversal):
    - If the right subtree of the predecessor node `pre` is empty, add the current node value to the result list $ans$, then point the right subtree of the predecessor node to the current node `root`, and update the current node to `root.left`.
    - If the right subtree of the predecessor node `pre` is not empty, point the right subtree of the predecessor node to null (i.e., disconnect `pre` and `root`), and update the current node to `root.right`.
1. Repeat the above steps until the binary tree node is null, and the traversal ends.

The time complexity is $O(n)$, where $n$ is the number of nodes in the binary tree. The space complexity is $O(1)$.

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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        while (root != null) {
            if (root.left == null) {
                ans.add(root.val);
                root = root.right;
            } else {
                TreeNode prev = root.left;
                while (prev.right != null && prev.right != root) {
                    prev = prev.right;
                }
                if (prev.right == null) {
                    ans.add(root.val);
                    prev.right = root;
                    root = root.left;
                } else {
                    prev.right = null;
                    root = root.right;
                }
            }
        }
        return ans;
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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        while (root) {
            if (!root->left) {
                ans.push_back(root->val);
                root = root->right;
            } else {
                TreeNode* prev = root->left;
                while (prev->right && prev->right != root) {
                    prev = prev->right;
                }
                if (!prev->right) {
                    ans.push_back(root->val);
                    prev->right = root;
                    root = root->left;
                } else {
                    prev->right = nullptr;
                    root = root->right;
                }
            }
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->