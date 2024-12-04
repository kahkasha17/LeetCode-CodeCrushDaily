# [2800. Shortest String That Contains Three Strings](https://leetcode.com/problems/shortest-string-that-contains-three-strings)

## Description

<!-- description:start -->

Given three strings <code>a</code>, <code>b</code>, and <code>c</code>, your task is to find a string that has the<strong> minimum</strong> length and contains all three strings as <strong>substrings</strong>.

<p>If there are multiple such strings, return the<em> </em><strong>lexicographically<em> </em>smallest </strong>one.</p>

<p>Return <em>a string denoting the answer to the problem.</em></p>

<p><strong>Notes</strong></p>

<ul>
	<li>A string <code>a</code> is <strong>lexicographically smaller</strong> than a string <code>b</code> (of the same length) if in the first position where <code>a</code> and <code>b</code> differ, string <code>a</code> has a letter that appears <strong>earlier </strong>in the alphabet than the corresponding letter in <code>b</code>.</li>
	<li>A <strong>substring</strong> is a contiguous sequence of characters within a string.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;abc&quot;, b = &quot;bca&quot;, c = &quot;aaa&quot;
<strong>Output:</strong> &quot;aaabca&quot;
<strong>Explanation:</strong>  We show that &quot;aaabca&quot; contains all the given strings: a = ans[2...4], b = ans[3..5], c = ans[0..2]. It can be shown that the length of the resulting string would be at least 6 and &quot;aaabca&quot; is the lexicographically smallest one.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;ab&quot;, b = &quot;ba&quot;, c = &quot;aba&quot;
<strong>Output:</strong> &quot;aba&quot;
<strong>Explanation: </strong>We show that the string &quot;aba&quot; contains all the given strings: a = ans[0..1], b = ans[1..2], c = ans[0..2]. Since the length of c is 3, the length of the resulting string would be at least 3. It can be shown that &quot;aba&quot; is the lexicographically smallest one.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length, c.length &lt;= 100</code></li>
	<li><code>a</code>, <code>b</code>, <code>c</code> consist only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We enumerate all permutations of the three strings, and for each permutation, we merge the three strings to find the shortest string with the smallest lexicographical order.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Where $n$ is the maximum length of the three strings.

#### Python3

```python
class Solution:
    def minimumString(self, a: str, b: str, c: str) -> str:
        def f(s: str, t: str) -> str:
            if s in t:
                return t
            if t in s:
                return s
            m, n = len(s), len(t)
            for i in range(min(m, n), 0, -1):
                if s[-i:] == t[:i]:
                    return s + t[i:]
            return s + t

        ans = ""
        for a, b, c in permutations((a, b, c)):
            s = f(f(a, b), c)
            if ans == "" or len(s) < len(ans) or (len(s) == len(ans) and s < ans):
                ans = s
        return ans
```

#### Java

```java
class Solution {
    public String minimumString(String a, String b, String c) {
        String[] s = {a, b, c};
        int[][] perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        String ans = "";
        for (var p : perm) {
            int i = p[0], j = p[1], k = p[2];
            String t = f(f(s[i], s[j]), s[k]);
            if ("".equals(ans) || t.length() < ans.length()
                || (t.length() == ans.length() && t.compareTo(ans) < 0)) {
                ans = t;
            }
        }
        return ans;
    }

    private String f(String s, String t) {
        if (s.contains(t)) {
            return s;
        }
        if (t.contains(s)) {
            return t;
        }
        int m = s.length(), n = t.length();
        for (int i = Math.min(m, n); i > 0; --i) {
            if (s.substring(m - i).equals(t.substring(0, i))) {
                return s + t.substring(i);
            }
        }
        return s + t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimumString(string a, string b, string c) {
        vector<string> s = {a, b, c};
        vector<vector<int>> perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        string ans = "";
        for (auto& p : perm) {
            int i = p[0], j = p[1], k = p[2];
            string t = f(f(s[i], s[j]), s[k]);
            if (ans == "" || t.size() < ans.size() || (t.size() == ans.size() && t < ans)) {
                ans = t;
            }
        }
        return ans;
    }

    string f(string s, string t) {
        if (s.find(t) != string::npos) {
            return s;
        }
        if (t.find(s) != string::npos) {
            return t;
        }
        int m = s.size(), n = t.size();
        for (int i = min(m, n); i; --i) {
            if (s.substr(m - i) == t.substr(0, i)) {
                return s + t.substr(i);
            }
        }
        return s + t;
    };
};
```
### Solution 2: Enumeration + KMP

We can use the KMP algorithm to optimize the string merging process.

Time complexity is $O(n)$, and space complexity is $O(n)$. Here, $n$ is the sum of the lengths of the three strings.

#### Python3

```python
class Solution:
    def minimumString(self, a: str, b: str, c: str) -> str:
        def f(s: str, t: str) -> str:
            if s in t:
                return t
            if t in s:
                return s
            p = t + "#" + s + "$"
            n = len(p)
            next = [0] * n
            next[0] = -1
            i, j = 2, 0
            while i < n:
                if p[i - 1] == p[j]:
                    j += 1
                    next[i] = j
                    i += 1
                elif j:
                    j = next[j]
                else:
                    next[i] = 0
                    i += 1
            return s + t[next[-1] :]

        ans = ""
        for a, b, c in permutations((a, b, c)):
            s = f(f(a, b), c)
            if ans == "" or len(s) < len(ans) or (len(s) == len(ans) and s < ans):
                ans = s
        return ans
```

#### Java

```java
class Solution {
    public String minimumString(String a, String b, String c) {
        String[] s = {a, b, c};
        int[][] perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        String ans = "";
        for (var p : perm) {
            int i = p[0], j = p[1], k = p[2];
            String t = f(f(s[i], s[j]), s[k]);
            if ("".equals(ans) || t.length() < ans.length()
                || (t.length() == ans.length() && t.compareTo(ans) < 0)) {
                ans = t;
            }
        }
        return ans;
    }

    private String f(String s, String t) {
        if (s.contains(t)) {
            return s;
        }
        if (t.contains(s)) {
            return t;
        }
        char[] p = (t + "#" + s + "$").toCharArray();
        int n = p.length;
        int[] next = new int[n];
        next[0] = -1;
        for (int i = 2, j = 0; i < n;) {
            if (p[i - 1] == p[j]) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s + t.substring(next[n - 1]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minimumString(string a, string b, string c) {
        vector<string> s = {a, b, c};
        vector<vector<int>> perm = {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}};
        string ans = "";
        for (auto& p : perm) {
            int i = p[0], j = p[1], k = p[2];
            string t = f(f(s[i], s[j]), s[k]);
            if (ans == "" || t.size() < ans.size() || (t.size() == ans.size() && t < ans)) {
                ans = t;
            }
        }
        return ans;
    }

    string f(string s, string t) {
        if (s.find(t) != string::npos) {
            return s;
        }
        if (t.find(s) != string::npos) {
            return t;
        }
        string p = t + "#" + s + "$";
        int n = p.size();
        int next[n];
        next[0] = -1;
        next[1] = 0;
        for (int i = 2, j = 0; i < n;) {
            if (p[i - 1] == p[j]) {
                next[i++] = ++j;
            } else if (j > 0) {
                j = next[j];
            } else {
                next[i++] = 0;
            }
        }
        return s + t.substr(next[n - 1]);
    };
};
```

<!-- solution:end -->

<!-- problem:end -->