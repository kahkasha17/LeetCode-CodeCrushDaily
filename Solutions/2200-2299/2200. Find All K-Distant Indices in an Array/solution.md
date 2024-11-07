# [2200. Find All K-Distant Indices in an Array](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array)

## Description

<!-- description:start -->

<p>You are given a <strong>0-indexed</strong> integer array <code>nums</code> and two integers <code>key</code> and <code>k</code>. A <strong>k-distant index</strong> is an index <code>i</code> of <code>nums</code> for which there exists at least one index <code>j</code> such that <code>|i - j| &lt;= k</code> and <code>nums[j] == key</code>.</p>

<p>Return <em>a list of all k-distant indices sorted in <strong>increasing order</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,4,9,1,3,9,5], key = 9, k = 1
<strong>Output:</strong> [1,2,3,4,5,6]
<strong>Explanation:</strong> Here, <code>nums[2] == key</code> and <code>nums[5] == key.
- For index 0, |0 - 2| &gt; k and |0 - 5| &gt; k, so there is no j</code> where <code>|0 - j| &lt;= k</code> and <code>nums[j] == key. Thus, 0 is not a k-distant index.
- For index 1, |1 - 2| &lt;= k and nums[2] == key, so 1 is a k-distant index.
- For index 2, |2 - 2| &lt;= k and nums[2] == key, so 2 is a k-distant index.
- For index 3, |3 - 2| &lt;= k and nums[2] == key, so 3 is a k-distant index.
- For index 4, |4 - 5| &lt;= k and nums[5] == key, so 4 is a k-distant index.
- For index 5, |5 - 5| &lt;= k and nums[5] == key, so 5 is a k-distant index.
- For index 6, |6 - 5| &lt;= k and nums[5] == key, so 6 is a k-distant index.
</code>Thus, we return [1,2,3,4,5,6] which is sorted in increasing order. 
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,2,2,2,2], key = 2, k = 2
<strong>Output:</strong> [0,1,2,3,4]
<strong>Explanation:</strong> For all indices i in nums, there exists some index j such that |i - j| &lt;= k and nums[j] == key, so every index is a k-distant index. 
Hence, we return [0,1,2,3,4].
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>key</code> is an integer from the array <code>nums</code>.</li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We enumerate the index $i$ in the range $[0, n)$, and for each index $i$, we enumerate the index $j$ in the range $[0, n)$. If $|i - j| \leq k$ and $nums[j] = key$, then $i$ is a K-nearest neighbor index. We add $i$ to the answer array, then break the inner loop and enumerate the next index $i$.

The time complexity is $O(n^2)$, where $n$ is the length of the array $nums$. The space complexity is $O(1)$.

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (Math.abs(i - j) <= k && nums[j] == key) {
                    ans.add(i);
                    break;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        int n = nums.size();
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (abs(i - j) <= k && nums[j] == key) {
                    ans.push_back(i);
                    break;
                }
            }
        }
        return ans;
    }
};
```
### Solution 2: Preprocessing + Binary Search

We can preprocess to get the indices of all elements equal to $key$, recorded in the array $idx$. All index elements in the array $idx$ are sorted in ascending order.

Next, we enumerate the index $i$. For each index $i$, we can use binary search to find elements in the range $[i - k, i + k]$ in the array $idx$. If there are elements, then $i$ is a K-nearest neighbor index. We add $i$ to the answer array.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        List<Integer> idx = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == key) {
                idx.add(i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; ++i) {
            int l = Collections.binarySearch(idx, i - k);
            int r = Collections.binarySearch(idx, i + k + 1);
            l = l < 0 ? -l - 1 : l;
            r = r < 0 ? -r - 2 : r - 1;
            if (l <= r) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        vector<int> idx;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] == key) {
                idx.push_back(i);
            }
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            auto it1 = lower_bound(idx.begin(), idx.end(), i - k);
            auto it2 = upper_bound(idx.begin(), idx.end(), i + k) - 1;
            if (it1 <= it2) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```
### Solution 3: Two Pointers

We enumerate the index $i$, and use a pointer $j$ to point to the smallest index that satisfies $j \geq i - k$ and $nums[j] = key$. If $j$ exists and $j \leq i + k$, then $i$ is a K-nearest neighbor index. We add $i$ to the answer array.

The time complexity is $O(n)$, where $n$ is the length of the array $nums$. The space complexity is $O(1)$.

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < i - k || (j < n && nums[j] != key)) {
                ++j;
            }
            if (j < n && j <= i + k) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        int n = nums.size();
        vector<int> ans;
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < i - k || (j < n && nums[j] != key)) {
                ++j;
            }
            if (j < n && j <= i + k) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->