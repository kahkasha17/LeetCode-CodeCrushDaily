# [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer)

## Description

<!-- description:start -->

<p>A linked list of length <code>n</code> is given such that each node contains an additional random pointer, which could point to any node in the list, or <code>null</code>.</p>

<p>Construct a <a href="https://en.wikipedia.org/wiki/Object_copying#Deep_copy" target="_blank"><strong>deep copy</strong></a> of the list. The deep copy should consist of exactly <code>n</code> <strong>brand new</strong> nodes, where each new node has its value set to the value of its corresponding original node. Both the <code>next</code> and <code>random</code> pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. <strong>None of the pointers in the new list should point to nodes in the original list</strong>.</p>

<p>For example, if there are two nodes <code>X</code> and <code>Y</code> in the original list, where <code>X.random --&gt; Y</code>, then for the corresponding two nodes <code>x</code> and <code>y</code> in the copied list, <code>x.random --&gt; y</code>.</p>

<p>Return <em>the head of the copied linked list</em>.</p>

<p>The linked list is represented in the input/output as a list of <code>n</code> nodes. Each node is represented as a pair of <code>[val, random_index]</code> where:</p>

<ul>
	<li><code>val</code>: an integer representing <code>Node.val</code></li>
	<li><code>random_index</code>: the index of the node (range from <code>0</code> to <code>n-1</code>) that the <code>random</code> pointer points to, or <code>null</code> if it does not point to any node.</li>
</ul>

<p>Your code will <strong>only</strong> be given the <code>head</code> of the original linked list.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0138.Copy%20List%20with%20Random%20Pointer/images/e1.png" style="width: 700px; height: 142px;" />
<pre>
<strong>Input:</strong> head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
<strong>Output:</strong> [[7,null],[13,0],[11,4],[10,2],[1,0]]
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0138.Copy%20List%20with%20Random%20Pointer/images/e2.png" style="width: 700px; height: 114px;" />
<pre>
<strong>Input:</strong> head = [[1,1],[2,1]]
<strong>Output:</strong> [[1,1],[2,1]]
</pre>

<p><strong class="example">Example 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0100-0199/0138.Copy%20List%20with%20Random%20Pointer/images/e3.png" style="width: 700px; height: 122px;" /></strong></p>

<pre>
<strong>Input:</strong> head = [[3,null],[3,0],[3,null]]
<strong>Output:</strong> [[3,null],[3,0],[3,null]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
	<li><code>-10<sup>4</sup> &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li><code>Node.random</code> is <code>null</code> or is pointing to some node in the linked list.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Java

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        Map<Node, Node> d = new HashMap<>();
        Node dummy = new Node(0);
        Node tail = dummy;
        for (Node cur = head; cur != null; cur = cur.next) {
            tail.next = new Node(cur.val);
            tail = tail.next;
            d.put(cur, tail);
        }
        tail = dummy.next;
        for (Node cur = head; cur != null; cur = cur.next) {
            tail.random = d.get(cur.random);
            tail = tail.next;
        }
        return dummy.next;
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        unordered_map<Node*, Node*> d;
        Node* dummy = new Node(0);
        Node* tail = dummy;
        for (auto cur = head; cur; cur = cur->next) {
            tail->next = new Node(cur->val);
            tail = tail->next;
            d[cur] = tail;
        }
        tail = dummy->next;
        for (auto cur = head; cur; cur = cur->next) {
            tail->random = d[cur->random];
            tail = tail->next;
        }
        return dummy->next;
    }
};
```
#### JavaScript

```js
/**
 * // Definition for a Node.
 * function Node(val, next, random) {
 *    this.val = val;
 *    this.next = next;
 *    this.random = random;
 * };
 */

/**
 * @param {Node} head
 * @return {Node}
 */
var copyRandomList = function (head) {
    const d = new Map();
    const dummy = new Node(0);
    let tail = dummy;
    for (let cur = head; cur; cur = cur.next) {
        tail.next = new Node(cur.val);
        tail = tail.next;
        d.set(cur, tail);
    }
    tail = dummy.next;
    for (let cur = head; cur; cur = cur.next) {
        tail.random = d.get(cur.random);
        tail = tail.next;
    }
    return dummy.next;
};
```

### Solution 2

#### Java

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        for (Node cur = head; cur != null;) {
            Node node = new Node(cur.val, cur.next);
            cur.next = node;
            cur = node.next;
        }
        for (Node cur = head; cur != null; cur = cur.next.next) {
            if (cur.random != null) {
                cur.next.random = cur.random.next;
            }
        }
        Node ans = head.next;
        for (Node cur = head; cur != null;) {
            Node nxt = cur.next;
            if (nxt != null) {
                cur.next = nxt.next;
            }
            cur = nxt;
        }
        return ans;
    }
}
```

#### C++

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;

    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) {
            return nullptr;
        }
        for (Node* cur = head; cur;) {
            Node* node = new Node(cur->val);
            node->next = cur->next;
            cur->next = node;
            cur = node->next;
        }
        for (Node* cur = head; cur; cur = cur->next->next) {
            if (cur->random) {
                cur->next->random = cur->random->next;
            }
        }
        Node* ans = head->next;
        for (Node* cur = head; cur;) {
            Node* nxt = cur->next;
            if (nxt) {
                cur->next = nxt->next;
            }
            cur = nxt;
        }
        return ans;
    }
};
```
#### JavaScript

```js
/**
 * // Definition for a Node.
 * function Node(val, next, random) {
 *    this.val = val;
 *    this.next = next;
 *    this.random = random;
 * };
 */

/**
 * @param {Node} head
 * @return {Node}
 */
var copyRandomList = function (head) {
    if (!head) {
        return null;
    }
    for (let cur = head; cur; ) {
        const node = new Node(cur.val, cur.next, null);
        cur.next = node;
        cur = node.next;
    }
    for (let cur = head; cur; cur = cur.next.next) {
        if (cur.random) {
            cur.next.random = cur.random.next;
        }
    }
    const ans = head.next;
    for (let cur = head; cur; ) {
        const nxt = cur.next;
        if (nxt) {
            cur.next = nxt.next;
        }
        cur = nxt;
    }
    return ans;
};
```

<!-- solution:end -->

<!-- problem:end -->