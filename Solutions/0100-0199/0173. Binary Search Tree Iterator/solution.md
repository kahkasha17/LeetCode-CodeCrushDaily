# [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator)

## Description

<!-- description:start -->

<p>Implement the <code>BSTIterator</code> class that represents an iterator over the <strong><a href="https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR)" target="_blank">in-order traversal</a></strong> of a binary search tree (BST):</p>

<ul>
	<li><code>BSTIterator(TreeNode root)</code> Initializes an object of the <code>BSTIterator</code> class. The <code>root</code> of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.</li>
	<li><code>boolean hasNext()</code> Returns <code>true</code> if there exists a number in the traversal to the right of the pointer, otherwise returns <code>false</code>.</li>
	<li><code>int next()</code> Moves the pointer to the right, then returns the number at the pointer.</li>
</ul>

<p>Notice that by initializing the pointer to a non-existent smallest number, the first call to <code>next()</code> will return the smallest element in the BST.</p>

<p>You may assume that <code>next()</code> calls will always be valid. That is, there will be at least a next number in the in-order traversal when <code>next()</code> is called.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0173.Binary%20Search%20Tree%20Iterator/images/bst-tree.png" style="width: 189px; height: 178px;" />
<pre>
<strong>Input</strong>
[&quot;BSTIterator&quot;, &quot;next&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;hasNext&quot;]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
<strong>Output</strong>
[null, 3, 7, true, 9, true, 15, true, 20, false]

<strong>Explanation</strong>
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next(); // return 3
bSTIterator.next(); // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next(); // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next(); // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next(); // return 20
bSTIterator.hasNext(); // return False

</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>5</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>6</sup></code></li>
	<li>At most <code>10<sup>5</sup></code> calls will be made to <code>hasNext</code>, and <code>next</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong></p>

<ul>
	<li>Could you implement <code>next()</code> and <code>hasNext()</code> to run in average <code>O(1)</code> time and use&nbsp;<code>O(h)</code> memory, where <code>h</code> is the height of the tree?</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

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
class BSTIterator {
    private int cur = 0;
    private List<Integer> vals = new ArrayList<>();

    public BSTIterator(TreeNode root) {
        inorder(root);
    }

    public int next() {
        return vals.get(cur++);
    }

    public boolean hasNext() {
        return cur < vals.size();
    }

    private void inorder(TreeNode root) {
        if (root != null) {
            inorder(root.left);
            vals.add(root.val);
            inorder(root.right);
        }
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
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
class BSTIterator {
public:
    vector<int> vals;
    int cur;
    BSTIterator(TreeNode* root) {
        cur = 0;
        inorder(root);
    }

    int next() {
        return vals[cur++];
    }

    bool hasNext() {
        return cur < vals.size();
    }

    void inorder(TreeNode* root) {
        if (root) {
            inorder(root->left);
            vals.push_back(root->val);
            inorder(root->right);
        }
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
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
 */
var BSTIterator = function (root) {
    this.stack = [];
    for (; root != null; root = root.left) {
        this.stack.push(root);
    }
};

/**
 * @return {number}
 */
BSTIterator.prototype.next = function () {
    let cur = this.stack.pop();
    let node = cur.right;
    for (; node != null; node = node.left) {
        this.stack.push(node);
    }
    return cur.val;
};

/**
 * @return {boolean}
 */
BSTIterator.prototype.hasNext = function () {
    return this.stack.length > 0;
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * var obj = new BSTIterator(root)
 * var param_1 = obj.next()
 * var param_2 = obj.hasNext()
 */
```

### Solution 2

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
class BSTIterator {
    private Deque<TreeNode> stack = new LinkedList<>();

    public BSTIterator(TreeNode root) {
        for (; root != null; root = root.left) {
            stack.offerLast(root);
        }
    }

    public int next() {
        TreeNode cur = stack.pollLast();
        for (TreeNode node = cur.right; node != null; node = node.left) {
            stack.offerLast(node);
        }
        return cur.val;
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }
}

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator obj = new BSTIterator(root);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
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
class BSTIterator {
public:
    stack<TreeNode*> stack;
    BSTIterator(TreeNode* root) {
        for (; root != nullptr; root = root->left) {
            stack.push(root);
        }
    }

    int next() {
        TreeNode* cur = stack.top();
        stack.pop();
        TreeNode* node = cur->right;
        for (; node != nullptr; node = node->left) {
            stack.push(node);
        }
        return cur->val;
    }

    bool hasNext() {
        return !stack.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```
#### Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
struct BSTIterator {
    stack: Vec<Option<Rc<RefCell<TreeNode>>>>,
}

use std::cell::RefCell;
use std::rc::Rc;
/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl BSTIterator {
    fn dfs(
        mut root: Option<Rc<RefCell<TreeNode>>>,
        stack: &mut Vec<Option<Rc<RefCell<TreeNode>>>>,
    ) {
        if root.is_some() {
            let left = root.as_mut().unwrap().borrow_mut().left.take();
            stack.push(root);
            Self::dfs(left, stack);
        }
    }

    fn new(root: Option<Rc<RefCell<TreeNode>>>) -> Self {
        let mut stack = vec![];
        Self::dfs(root, &mut stack);
        BSTIterator { stack }
    }

    fn next(&mut self) -> i32 {
        let node = self.stack.pop().unwrap().unwrap();
        let mut node = node.borrow_mut();
        if node.right.is_some() {
            Self::dfs(node.right.take(), &mut self.stack);
        }
        node.val
    }

    fn has_next(&self) -> bool {
        self.stack.len() != 0
    }
}
```
<!-- solution:end -->

<!-- problem:end -->