# [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii)

## Description

<!-- description:start -->

<p>Find all valid combinations of <code>k</code> numbers that sum up to <code>n</code> such that the following conditions are true:</p>

<ul>
	<li>Only numbers <code>1</code> through <code>9</code> are used.</li>
	<li>Each number is used <strong>at most once</strong>.</li>
</ul>

<p>Return <em>a list of all possible valid combinations</em>. The list must not contain the same combination twice, and the combinations may be returned in any order.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> k = 3, n = 7
<strong>Output:</strong> [[1,2,4]]
<strong>Explanation:</strong>
1 + 2 + 4 = 7
There are no other valid combinations.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> k = 3, n = 9
<strong>Output:</strong> [[1,2,6],[1,3,5],[2,3,4]]
<strong>Explanation:</strong>
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
There are no other valid combinations.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> k = 4, n = 1
<strong>Output:</strong> []
<strong>Explanation:</strong> There are no valid combinations.
Using 4 different numbers in the range [1,9], the smallest sum we can get is 1+2+3+4 = 10 and since 10 &gt; 1, there are no valid combination.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= k &lt;= 9</code></li>
	<li><code>1 &lt;= n &lt;= 60</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Pruning + Backtracking (Two Approaches)

We design a function $dfs(i, s)$, which represents that we are currently enumerating the number $i$, and there are still numbers with a sum of $s$ to be enumerated. The current search path is $t$, and the answer is $ans$.

The execution logic of the function $dfs(i, s)$ is as follows:

Approach One:

-   If $s = 0$ and the length of the current search path $t$ is $k$, it means that a group of answers has been found. Add $t$ to $ans$ and then return.
-   If $i \gt 9$ or $i \gt s$ or the length of the current search path $t$ is greater than $k$, it means that the current search path cannot be the answer, so return directly.
-   Otherwise, we can choose to add the number $i$ to the search path $t$, and then continue to search, i.e., execute $dfs(i + 1, s - i)$. After the search is completed, remove $i$ from the search path $t$; we can also choose not to add the number $i$ to the search path $t$, and directly execute $dfs(i + 1, s)$.

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int k;

    public List<List<Integer>> combinationSum3(int k, int n) {
        this.k = k;
        dfs(1, n);
        return ans;
    }

    private void dfs(int i, int s) {
        if (s == 0) {
            if (t.size() == k) {
                ans.add(new ArrayList<>(t));
            }
            return;
        }
        if (i > 9 || i > s || t.size() >= k) {
            return;
        }
        t.add(i);
        dfs(i + 1, s - i);
        t.remove(t.size() - 1);
        dfs(i + 1, s);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        vector<int> t;
        function<void(int, int)> dfs = [&](int i, int s) {
            if (s == 0) {
                if (t.size() == k) {
                    ans.emplace_back(t);
                }
                return;
            }
            if (i > 9 || i > s || t.size() >= k) {
                return;
            }
            t.emplace_back(i);
            dfs(i + 1, s - i);
            t.pop_back();
            dfs(i + 1, s);
        };
        dfs(1, n);
        return ans;
    }
};
```
#### JavaScript

```js
function combinationSum3(k, n) {
    const ans = [];
    const t = [];
    const dfs = (i, s) => {
        if (s === 0) {
            if (t.length === k) {
                ans.push(t.slice());
            }
            return;
        }
        if (i > 9 || i > s || t.length >= k) {
            return;
        }
        t.push(i);
        dfs(i + 1, s - i);
        t.pop();
        dfs(i + 1, s);
    };
    dfs(1, n);
    return ans;
}
```
Another approach:

-   If $s = 0$ and the length of the current search path $t$ is $k$, it means that a group of answers has been found. Add $t$ to $ans$ and then return.
-   If $i \gt 9$ or $i \gt s$ or the length of the current search path $t$ is greater than $k$, it means that the current search path cannot be the answer, so return directly.
-   Otherwise, we enumerate the next number $j$, i.e., $j \in [i, 9]$, add the number $j$ to the search path $t$, and then continue to search, i.e., execute $dfs(j + 1, s - j)$. After the search is completed, remove $j$ from the search path $t$.

In the main function, we call $dfs(1, n)$, i.e., start enumerating from the number $1$, and the remaining numbers with a sum of $n$ need to be enumerated. After the search is completed, we can get all the answers.

The time complexity is $(C_{9}^k \times k)$, and the space complexity is $O(k)$.

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private List<Integer> t = new ArrayList<>();
    private int k;

    public List<List<Integer>> combinationSum3(int k, int n) {
        this.k = k;
        dfs(1, n);
        return ans;
    }

    private void dfs(int i, int s) {
        if (s == 0) {
            if (t.size() == k) {
                ans.add(new ArrayList<>(t));
            }
            return;
        }
        if (i > 9 || i > s || t.size() >= k) {
            return;
        }
        for (int j = i; j <= 9; ++j) {
            t.add(j);
            dfs(j + 1, s - j);
            t.remove(t.size() - 1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        vector<int> t;
        function<void(int, int)> dfs = [&](int i, int s) {
            if (s == 0) {
                if (t.size() == k) {
                    ans.emplace_back(t);
                }
                return;
            }
            if (i > 9 || i > s || t.size() >= k) {
                return;
            }
            for (int j = i; j <= 9; ++j) {
                t.emplace_back(j);
                dfs(j + 1, s - j);
                t.pop_back();
            }
        };
        dfs(1, n);
        return ans;
    }
};
```
#### JavaScript

```js
function combinationSum3(k, n) {
    const ans = [];
    const t = [];
    const dfs = (i, s) => {
        if (s === 0) {
            if (t.length === k) {
                ans.push(t.slice());
            }
            return;
        }
        if (i > 9 || i > s || t.length >= k) {
            return;
        }
        for (let j = i; j <= 9; ++j) {
            t.push(j);
            dfs(j + 1, s - j);
            t.pop();
        }
    };
    dfs(1, n);
    return ans;
}
```
### Solution 2: Binary Enumeration

We can use a binary integer of length $9$ to represent the selection of numbers $1$ to $9$, where the $i$-th bit of the binary integer represents whether the number $i + 1$ is selected. If the $i$-th bit is $1$, it means that the number $i + 1$ is selected, otherwise, it means that the number $i + 1$ is not selected.

We enumerate binary integers in the range of $[0, 2^9)$. For the currently enumerated binary integer $mask$, if the number of $1$s in the binary representation of $mask$ is $k$, and the sum of the numbers corresponding to $1$ in the binary representation of $mask$ is $n$, it means that the number selection scheme corresponding to $mask$ is a group of answers. We can add the number selection scheme corresponding to $mask$ to the answer.

The time complexity is $O(2^9 \times 9)$, and the space complexity is $O(k)$.
#### Java

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        for (int mask = 0; mask < 1 << 9; ++mask) {
            if (Integer.bitCount(mask) == k) {
                List<Integer> t = new ArrayList<>();
                int s = 0;
                for (int i = 0; i < 9; ++i) {
                    if ((mask >> i & 1) == 1) {
                        s += (i + 1);
                        t.add(i + 1);
                    }
                }
                if (s == n) {
                    ans.add(t);
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
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> ans;
        for (int mask = 0; mask < 1 << 9; ++mask) {
            if (__builtin_popcount(mask) == k) {
                int s = 0;
                vector<int> t;
                for (int i = 0; i < 9; ++i) {
                    if (mask >> i & 1) {
                        t.push_back(i + 1);
                        s += i + 1;
                    }
                }
                if (s == n) {
                    ans.emplace_back(t);
                }
            }
        }
        return ans;
    }
};
```
#### JavaScript

```js
function combinationSum3(k, n) {
    const ans = [];
    for (let mask = 0; mask < 1 << 9; ++mask) {
        if (bitCount(mask) === k) {
            const t = [];
            let s = 0;
            for (let i = 0; i < 9; ++i) {
                if (mask & (1 << i)) {
                    t.push(i + 1);
                    s += i + 1;
                }
            }
            if (s === n) {
                ans.push(t);
            }
        }
    }
    return ans;
}

function bitCount(i) {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- solution:end -->

<!-- problem:end -->