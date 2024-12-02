# [1455. Check If a Word Occurs As a Prefix of Any Word in a Sentence](https://leetcode.com/problems/check-if-a-word-occurs-as-a-prefix-of-any-word-in-a-sentence)

## Description

<!-- description:start -->

<p>Given a <code>sentence</code> that consists of some words separated by a <strong>single space</strong>, and a <code>searchWord</code>, check if <code>searchWord</code> is a prefix of any word in <code>sentence</code>.</p>

<p>Return <em>the index of the word in </em><code>sentence</code><em> (<strong>1-indexed</strong>) where </em><code>searchWord</code><em> is a prefix of this word</em>. If <code>searchWord</code> is a prefix of more than one word, return the index of the first word <strong>(minimum index)</strong>. If there is no such word return <code>-1</code>.</p>

<p>A <strong>prefix</strong> of a string <code>s</code> is any leading contiguous substring of <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> sentence = &quot;i love eating burger&quot;, searchWord = &quot;burg&quot;
<strong>Output:</strong> 4
<strong>Explanation:</strong> &quot;burg&quot; is prefix of &quot;burger&quot; which is the 4th word in the sentence.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> sentence = &quot;this problem is an easy problem&quot;, searchWord = &quot;pro&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> &quot;pro&quot; is prefix of &quot;problem&quot; which is the 2nd and the 6th word in the sentence, but we return 2 as it&#39;s the minimal index.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> sentence = &quot;i am tired&quot;, searchWord = &quot;you&quot;
<strong>Output:</strong> -1
<strong>Explanation:</strong> &quot;you&quot; is not a prefix of any word in the sentence.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 100</code></li>
	<li><code>1 &lt;= searchWord.length &lt;= 10</code></li>
	<li><code>sentence</code> consists of lowercase English letters and spaces.</li>
	<li><code>searchWord</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: String Splitting

We split $\textit{sentence}$ by spaces into $\textit{words}$, then iterate through $\textit{words}$ to check if $\textit{words}[i]$ is a prefix of $\textit{searchWord}$. If it is, we return $i+1$. If the iteration completes and no words satisfy the condition, we return $-1$.

The time complexity is $O(m \times n)$, and the space complexity is $O(m)$. Here, $m$ and $n$ are the lengths of $\textit{sentence}$ and $\textit{searchWord}$, respectively.

#### Python3

```python
class Solution:
    def isPrefixOfWord(self, sentence: str, searchWord: str) -> int:
        for i, s in enumerate(sentence.split(), 1):
            if s.startswith(searchWord):
                return i
        return -1
```

#### Java

```java
class Solution {
    public int isPrefixOfWord(String sentence, String searchWord) {
        String[] words = sentence.split(" ");
        for (int i = 0; i < words.length; ++i) {
            if (words[i].startsWith(searchWord)) {
                return i + 1;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int isPrefixOfWord(string sentence, string searchWord) {
        stringstream ss(sentence);
        string s;
        for (int i = 1; ss >> s; ++i) {
            if (s.find(searchWord) == 0) {
                return i;
            }
        }
        return -1;
    }
};
```
<!-- solution:end -->

<!-- problem:end -->