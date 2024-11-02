# [68. Text Justification](https://leetcode.com/problems/text-justification)

## Description

<!-- description:start -->

<p>Given an array of strings <code>words</code> and a width <code>maxWidth</code>, format the text such that each line has exactly <code>maxWidth</code> characters and is fully (left and right) justified.</p>

<p>You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces <code>&#39; &#39;</code> when necessary so that each line has exactly <code>maxWidth</code> characters.</p>

<p>Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.</p>

<p>For the last line of text, it should be left-justified, and no extra space is inserted between words.</p>

<p><strong>Note:</strong></p>

<ul>
	<li>A word is defined as a character sequence consisting of non-space characters only.</li>
	<li>Each word&#39;s length is guaranteed to be greater than <code>0</code> and not exceed <code>maxWidth</code>.</li>
	<li>The input array <code>words</code> contains at least one word.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;This&quot;, &quot;is&quot;, &quot;an&quot;, &quot;example&quot;, &quot;of&quot;, &quot;text&quot;, &quot;justification.&quot;], maxWidth = 16
<strong>Output:</strong>
[
&nbsp; &nbsp;&quot;This &nbsp; &nbsp;is &nbsp; &nbsp;an&quot;,
&nbsp; &nbsp;&quot;example &nbsp;of text&quot;,
&nbsp; &nbsp;&quot;justification. &nbsp;&quot;
]</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;What&quot;,&quot;must&quot;,&quot;be&quot;,&quot;acknowledgment&quot;,&quot;shall&quot;,&quot;be&quot;], maxWidth = 16
<strong>Output:</strong>
[
&nbsp; &quot;What &nbsp; must &nbsp; be&quot;,
&nbsp; &quot;acknowledgment &nbsp;&quot;,
&nbsp; &quot;shall be &nbsp; &nbsp; &nbsp; &nbsp;&quot;
]
<strong>Explanation:</strong> Note that the last line is &quot;shall be    &quot; instead of &quot;shall     be&quot;, because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> words = [&quot;Science&quot;,&quot;is&quot;,&quot;what&quot;,&quot;we&quot;,&quot;understand&quot;,&quot;well&quot;,&quot;enough&quot;,&quot;to&quot;,&quot;explain&quot;,&quot;to&quot;,&quot;a&quot;,&quot;computer.&quot;,&quot;Art&quot;,&quot;is&quot;,&quot;everything&quot;,&quot;else&quot;,&quot;we&quot;,&quot;do&quot;], maxWidth = 20
<strong>Output:</strong>
[
&nbsp; &quot;Science &nbsp;is &nbsp;what we&quot;,
  &quot;understand &nbsp; &nbsp; &nbsp;well&quot;,
&nbsp; &quot;enough to explain to&quot;,
&nbsp; &quot;a &nbsp;computer. &nbsp;Art is&quot;,
&nbsp; &quot;everything &nbsp;else &nbsp;we&quot;,
&nbsp; &quot;do &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&quot;
]</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 300</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li><code>words[i]</code> consists of only English letters and symbols.</li>
	<li><code>1 &lt;= maxWidth &lt;= 100</code></li>
	<li><code>words[i].length &lt;= maxWidth</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

We can simulate the process according to the problem's requirements. Note that if it is the last line, or if there is only one word in the line, then we should align to the left. Otherwise, we should distribute the spaces evenly.

The time complexity is $O(L)$, and the space complexity is $O(L)$. Here, $L$ is the sum of the lengths of all words.

#### Java

```java
class Solution {
    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> ans = new ArrayList<>();
        for (int i = 0, n = words.length; i < n;) {
            List<String> t = new ArrayList<>();
            t.add(words[i]);
            int cnt = words[i].length();
            ++i;
            while (i < n && cnt + 1 + words[i].length() <= maxWidth) {
                cnt += 1 + words[i].length();
                t.add(words[i++]);
            }
            if (i == n || t.size() == 1) {
                String left = String.join(" ", t);
                String right = " ".repeat(maxWidth - left.length());
                ans.add(left + right);
                continue;
            }
            int spaceWidth = maxWidth - (cnt - t.size() + 1);
            int w = spaceWidth / (t.size() - 1);
            int m = spaceWidth % (t.size() - 1);
            StringBuilder row = new StringBuilder();
            for (int j = 0; j < t.size() - 1; ++j) {
                row.append(t.get(j));
                row.append(" ".repeat(w + (j < m ? 1 : 0)));
            }
            row.append(t.get(t.size() - 1));
            ans.add(row.toString());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> ans;
        for (int i = 0, n = words.size(); i < n;) {
            vector<string> t = {words[i]};
            int cnt = words[i].size();
            ++i;
            while (i < n && cnt + 1 + words[i].size() <= maxWidth) {
                cnt += 1 + words[i].size();
                t.emplace_back(words[i++]);
            }
            if (i == n || t.size() == 1) {
                string left = t[0];
                for (int j = 1; j < t.size(); ++j) {
                    left += " " + t[j];
                }
                string right = string(maxWidth - left.size(), ' ');
                ans.emplace_back(left + right);
                continue;
            }
            int spaceWidth = maxWidth - (cnt - t.size() + 1);
            int w = spaceWidth / (t.size() - 1);
            int m = spaceWidth % (t.size() - 1);
            string row;
            for (int j = 0; j < t.size() - 1; ++j) {
                row += t[j] + string(w + (j < m ? 1 : 0), ' ');
            }
            row += t.back();
            ans.emplace_back(row);
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->