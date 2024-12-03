# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters)

## Description

<!-- description:start -->

<p>Given a string <code>s</code>, remove duplicate letters so that every letter appears once and only once. You must make sure your result is <span data-keyword="lexicographically-smaller-string"><strong>the smallest in lexicographical order</strong></span> among all possible results.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;bcabc&quot;
<strong>Output:</strong> &quot;abc&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;cbacdcbc&quot;
<strong>Output:</strong> &quot;acdb&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Note:</strong> This question is the same as 1081: <a href="https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/" target="_blank">https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/</a></p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Stack

We use an array `last` to record the last occurrence of each character, a stack to save the result string, and an array `vis` or an integer variable `mask` to record whether the current character is in the stack.

Traverse the string $s$, for each character $c$, if $c$ is not in the stack, we need to check whether the top element of the stack is greater than $c$. If it is greater than $c$ and the top element of the stack will appear later, we pop the top element of the stack and push $c$ into the stack.

Finally, concatenate the elements in the stack into a string and return it as the result.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the string $s$.

#### Python3

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        last = {c: i for i, c in enumerate(s)}
        stk = []
        vis = set()
        for i, c in enumerate(s):
            if c in vis:
                continue
            while stk and stk[-1] > c and last[stk[-1]] > i:
                vis.remove(stk.pop())
            stk.append(c)
            vis.add(c)
        return ''.join(stk)
```

#### Java

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int n = s.length();
        int[] last = new int[26];
        for (int i = 0; i < n; ++i) {
            last[s.charAt(i) - 'a'] = i;
        }
        Deque<Character> stk = new ArrayDeque<>();
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (((mask >> (c - 'a')) & 1) == 1) {
                continue;
            }
            while (!stk.isEmpty() && stk.peek() > c && last[stk.peek() - 'a'] > i) {
                mask ^= 1 << (stk.pop() - 'a');
            }
            stk.push(c);
            mask |= 1 << (c - 'a');
        }
        StringBuilder ans = new StringBuilder();
        for (char c : stk) {
            ans.append(c);
        }
        return ans.reverse().toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string removeDuplicateLetters(string s) {
        int n = s.size();
        int last[26] = {0};
        for (int i = 0; i < n; ++i) {
            last[s[i] - 'a'] = i;
        }
        string ans;
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            char c = s[i];
            if ((mask >> (c - 'a')) & 1) {
                continue;
            }
            while (!ans.empty() && ans.back() > c && last[ans.back() - 'a'] > i) {
                mask ^= 1 << (ans.back() - 'a');
                ans.pop_back();
            }
            ans.push_back(c);
            mask |= 1 << (c - 'a');
        }
        return ans;
    }
};
```
### Solution 2

#### Python3

```python
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        count, in_stack = [0] * 128, [False] * 128
        stack = []
        for c in s:
            count[ord(c)] += 1
        for c in s:
            count[ord(c)] -= 1
            if in_stack[ord(c)]:
                continue
            while len(stack) and stack[-1] > c:
                peek = stack[-1]
                if count[ord(peek)] < 1:
                    break
                in_stack[ord(peek)] = False
                stack.pop()
            stack.append(c)
            in_stack[ord(c)] = True
        return ''.join(stack)
```

<!-- solution:end -->

<!-- problem:end -->