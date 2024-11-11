# [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

## Description

<!-- description:start -->

<p>You are given an array of strings <code>tokens</code> that represents an arithmetic expression in a <a href="http://en.wikipedia.org/wiki/Reverse_Polish_notation" target="_blank">Reverse Polish Notation</a>.</p>

<p>Evaluate the expression. Return <em>an integer that represents the value of the expression</em>.</p>

<p><strong>Note</strong> that:</p>

<ul>
	<li>The valid operators are <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, <code>&#39;*&#39;</code>, and <code>&#39;/&#39;</code>.</li>
	<li>Each operand may be an integer or another expression.</li>
	<li>The division between two integers always <strong>truncates toward zero</strong>.</li>
	<li>There will not be any division by zero.</li>
	<li>The input represents a valid arithmetic expression in a reverse polish notation.</li>
	<li>The answer and all the intermediate calculations can be represented in a <strong>32-bit</strong> integer.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> tokens = [&quot;2&quot;,&quot;1&quot;,&quot;+&quot;,&quot;3&quot;,&quot;*&quot;]
<strong>Output:</strong> 9
<strong>Explanation:</strong> ((2 + 1) * 3) = 9
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> tokens = [&quot;4&quot;,&quot;13&quot;,&quot;5&quot;,&quot;/&quot;,&quot;+&quot;]
<strong>Output:</strong> 6
<strong>Explanation:</strong> (4 + (13 / 5)) = 6
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> tokens = [&quot;10&quot;,&quot;6&quot;,&quot;9&quot;,&quot;3&quot;,&quot;+&quot;,&quot;-11&quot;,&quot;*&quot;,&quot;/&quot;,&quot;*&quot;,&quot;17&quot;,&quot;+&quot;,&quot;5&quot;,&quot;+&quot;]
<strong>Output:</strong> 22
<strong>Explanation:</strong> ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= tokens.length &lt;= 10<sup>4</sup></code></li>
	<li><code>tokens[i]</code> is either an operator: <code>&quot;+&quot;</code>, <code>&quot;-&quot;</code>, <code>&quot;*&quot;</code>, or <code>&quot;/&quot;</code>, or an integer in the range <code>[-200, 200]</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Java

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (String t : tokens) {
            if (t.length() > 1 || Character.isDigit(t.charAt(0))) {
                stk.push(Integer.parseInt(t));
            } else {
                int y = stk.pop();
                int x = stk.pop();
                switch (t) {
                case "+":
                    stk.push(x + y);
                    break;
                case "-":
                    stk.push(x - y);
                    break;
                case "*":
                    stk.push(x * y);
                    break;
                default:
                    stk.push(x / y);
                    break;
                }
            }
        }
        return stk.pop();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        for (auto& t : tokens) {
            if (t.size() > 1 || isdigit(t[0])) {
                stk.push(stoi(t));
            } else {
                int y = stk.top();
                stk.pop();
                int x = stk.top();
                stk.pop();
                if (t[0] == '+')
                    stk.push(x + y);
                else if (t[0] == '-')
                    stk.push(x - y);
                else if (t[0] == '*')
                    stk.push(x * y);
                else
                    stk.push(x / y);
            }
        }
        return stk.top();
    }
};
```

<!-- solution:end -->

<!-- problem:end -->