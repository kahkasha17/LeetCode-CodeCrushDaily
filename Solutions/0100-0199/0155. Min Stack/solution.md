# [155. Min Stack](https://leetcode.com/problems/min-stack)

## Description

<!-- description:start -->

<p>Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.</p>

<p>Implement the <code>MinStack</code> class:</p>

<ul>
	<li><code>MinStack()</code> initializes the stack object.</li>
	<li><code>void push(int val)</code> pushes the element <code>val</code> onto the stack.</li>
	<li><code>void pop()</code> removes the element on the top of the stack.</li>
	<li><code>int top()</code> gets the top element of the stack.</li>
	<li><code>int getMin()</code> retrieves the minimum element in the stack.</li>
</ul>

<p>You must implement a solution with <code>O(1)</code> time complexity for each function.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;MinStack&quot;,&quot;push&quot;,&quot;push&quot;,&quot;push&quot;,&quot;getMin&quot;,&quot;pop&quot;,&quot;top&quot;,&quot;getMin&quot;]
[[],[-2],[0],[-3],[],[],[],[]]

<strong>Output</strong>
[null,null,null,null,-3,null,0,-2]

<strong>Explanation</strong>
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= val &lt;= 2<sup>31</sup> - 1</code></li>
	<li>Methods <code>pop</code>, <code>top</code> and <code>getMin</code> operations will always be called on <strong>non-empty</strong> stacks.</li>
	<li>At most <code>3 * 10<sup>4</sup></code> calls will be made to <code>push</code>, <code>pop</code>, <code>top</code>, and <code>getMin</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Java

```java
class MinStack {
    private Deque<Integer> stk1 = new ArrayDeque<>();
    private Deque<Integer> stk2 = new ArrayDeque<>();

    public MinStack() {
        stk2.push(Integer.MAX_VALUE);
    }

    public void push(int val) {
        stk1.push(val);
        stk2.push(Math.min(val, stk2.peek()));
    }

    public void pop() {
        stk1.pop();
        stk2.pop();
    }

    public int top() {
        return stk1.peek();
    }

    public int getMin() {
        return stk2.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(val);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

#### C++

```cpp
class MinStack {
public:
    MinStack() {
        stk2.push(INT_MAX);
    }

    void push(int val) {
        stk1.push(val);
        stk2.push(min(val, stk2.top()));
    }

    void pop() {
        stk1.pop();
        stk2.pop();
    }

    int top() {
        return stk1.top();
    }

    int getMin() {
        return stk2.top();
    }

private:
    stack<int> stk1;
    stack<int> stk2;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```
#### JavaScript

```js
var MinStack = function () {
    this.stk1 = [];
    this.stk2 = [Infinity];
};

/**
 * @param {number} val
 * @return {void}
 */
MinStack.prototype.push = function (val) {
    this.stk1.push(val);
    this.stk2.push(Math.min(this.stk2[this.stk2.length - 1], val));
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
    this.stk1.pop();
    this.stk2.pop();
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
    return this.stk1[this.stk1.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
    return this.stk2[this.stk2.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(val)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

<!-- solution:end -->

<!-- problem:end -->