<!-- problem:start -->

# [49. Group Anagrams](https://leetcode.com/problems/group-anagrams)


## Description

<!-- description:start -->

<p>Given an array of strings <code>strs</code>, group the <span data-keyword="anagram">anagrams</span> together. You can return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">strs = [&quot;eat&quot;,&quot;tea&quot;,&quot;tan&quot;,&quot;ate&quot;,&quot;nat&quot;,&quot;bat&quot;]</span></p>

<p><strong>Output:</strong> <span class="example-io">[[&quot;bat&quot;],[&quot;nat&quot;,&quot;tan&quot;],[&quot;ate&quot;,&quot;eat&quot;,&quot;tea&quot;]]</span></p>

<p><strong>Explanation:</strong></p>

<ul>
	<li>There is no string in strs that can be rearranged to form <code>&quot;bat&quot;</code>.</li>
	<li>The strings <code>&quot;nat&quot;</code> and <code>&quot;tan&quot;</code> are anagrams as they can be rearranged to form each other.</li>
	<li>The strings <code>&quot;ate&quot;</code>, <code>&quot;eat&quot;</code>, and <code>&quot;tea&quot;</code> are anagrams as they can be rearranged to form each other.</li>
</ul>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">strs = [&quot;&quot;]</span></p>

<p><strong>Output:</strong> <span class="example-io">[[&quot;&quot;]]</span></p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">strs = [&quot;a&quot;]</span></p>

<p><strong>Output:</strong> <span class="example-io">[[&quot;a&quot;]]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table

1. Traverse the string array, sort each string in **character dictionary order** to get a new string.
2. Use the new string as `key` and `[str]` as `value`, and store them in the hash table (`HashMap<String, List<String>>`).
3. When encountering the same `key` during subsequent traversal, add it to the corresponding `value`.

Take `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]` as an example. At the end of the traversal, the state of the hash table is:

| key     | value                   |
| ------- | ----------------------- |
| `"aet"` | `["eat", "tea", "ate"]` |
| `"ant"` | `["tan", "nat"] `       |
| `"abt"` | `["bat"] `              |

Finally, return the `value` list of the hash table.

The time complexity is $O(n\times k\times \log k)$, where $n$ and $k$ are the lengths of the string array and the maximum length of the string, respectively.


#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            char[] t = s.toCharArray();
            Arrays.sort(t);
            String k = String.valueOf(t);
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> d;
        for (auto& s : strs) {
            string k = s;
            sort(k.begin(), k.end());
            d[k].emplace_back(s);
        }
        vector<vector<string>> ans;
        for (auto& [_, v] : d) ans.emplace_back(v);
        return ans;
    }
};
```

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Counting

We can also change the sorting part in Solution 1 to counting, that is, use the characters in each string $s$ and their occurrence times as `key`, and use the string $s$ as `value` to store in the hash table.

The time complexity is $O(n\times (k + C))$, where $n$ and $k$ are the lengths of the string array and the maximum length of the string, respectively, and $C$ is the size of the character set. In this problem, $C = 26$.

#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            int[] cnt = new int[26];
            for (int i = 0; i < s.length(); ++i) {
                ++cnt[s.charAt(i) - 'a'];
            }
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0) {
                    sb.append((char) ('a' + i)).append(cnt[i]);
                }
            }
            String k = sb.toString();
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> d;
        for (auto& s : strs) {
            int cnt[26] = {0};
            for (auto& c : s) ++cnt[c - 'a'];
            string k;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i]) {
                    k += 'a' + i;
                    k += to_string(cnt[i]);
                }
            }
            d[k].emplace_back(s);
        }
        vector<vector<string>> ans;
        for (auto& [_, v] : d) ans.emplace_back(v);
        return ans;
    }
};
```


<!-- solution:end -->

<!-- problem:end -->