# [2604. Minimum Time to Eat All Grains 🔒](https://leetcode.com/problems/minimum-time-to-eat-all-grains)


## Description

<!-- description:start -->

<p>There are <code>n</code> hens and <code>m</code> grains on a line. You are given the initial positions of the hens and the grains in two integer arrays <code>hens</code> and <code>grains</code> of size <code>n</code> and <code>m</code> respectively.</p>

<p>Any hen can eat a grain if they are on the same position. The time taken for this is negligible. One hen can also eat multiple grains.</p>

<p>In <code>1</code> second, a hen can move right or left by <code>1</code> unit. The hens can move simultaneously and independently of each other.</p>

<p>Return <em>the <strong>minimum</strong> time to eat all grains if the hens act optimally.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> hens = [3,6,7], grains = [2,4,7,9]
<strong>Output:</strong> 2
<strong>Explanation:</strong> 
One of the ways hens eat all grains in 2 seconds is described below:
- The first hen eats the grain at position 2 in 1 second. 
- The second hen eats the grain at position 4 in 2 seconds. 
- The third hen eats the grains at positions 7 and 9 in 2 seconds. 
So, the maximum time needed is 2.
It can be proven that the hens cannot eat all grains before 2 seconds.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> hens = [4,6,109,111,213,215], grains = [5,110,214]
<strong>Output:</strong> 1
<strong>Explanation:</strong> 
One of the ways hens eat all grains in 1 second is described below:
- The first hen eats the grain at position 5 in 1 second. 
- The fourth hen eats the grain at position 110 in 1 second.
- The sixth hen eats the grain at position 214 in 1 second. 
- The other hens do not move. 
So, the maximum time needed is 1.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= hens.length, grains.length &lt;= 2*10<sup>4</sup></code></li>
	<li><code>0 &lt;= hens[i], grains[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Sorting + Binary Search

First, sort the chickens and grains by their position from left to right. Then enumerate the time $t$ using binary search to find the smallest $t$ such that all the grains can be eaten up in $t$ seconds.

For each chicken, we use the pointer $j$ to point to the leftmost grain that has not been eaten, and the current position of the chicken is $x$ and the position of the grain is $y$. There are the following cases:

-   If $y \leq x$, we note that $d = x - y$. If $d \gt t$, the current grain cannot be eaten, so directly return `false`. Otherwise, move the pointer $j$ to the right until $j=m$ or $grains[j] \gt x$. At this point, we need to check whether the chicken can eat the grain pointed to by $j$. If it can, continue to move the pointer $j$ to the right until $j=m$ or $min(d, grains[j] - x) + grains[j] - y \gt t$.
-   If $y \lt x$, move the pointer $j$ to the right until $j=m$ or $grains[j] - x \gt t$.

If $j=m$, it means that all the grains have been eaten, return `true`, otherwise return `false`.

Time complexity $O(n \times \log n + m \times \log m + (m + n) \times \log U)$, space complexity $O(\log m + \log n)$. $n$ and $m$ are the number of chickens and grains respectively, and $U$ is the maximum value of all the chicken and grain positions.

#### Java

```java
class Solution {
    private int[] hens;
    private int[] grains;
    private int m;

    public int minimumTime(int[] hens, int[] grains) {
        m = grains.length;
        this.hens = hens;
        this.grains = grains;
        Arrays.sort(hens);
        Arrays.sort(grains);
        int l = 0;
        int r = Math.abs(hens[0] - grains[0]) + grains[m - 1] - grains[0];
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int t) {
        int j = 0;
        for (int x : hens) {
            if (j == m) {
                return true;
            }
            int y = grains[j];
            if (y <= x) {
                int d = x - y;
                if (d > t) {
                    return false;
                }
                while (j < m && grains[j] <= x) {
                    ++j;
                }
                while (j < m && Math.min(d, grains[j] - x) + grains[j] - y <= t) {
                    ++j;
                }
            } else {
                while (j < m && grains[j] - x <= t) {
                    ++j;
                }
            }
        }
        return j == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(vector<int>& hens, vector<int>& grains) {
        int m = grains.size();
        sort(hens.begin(), hens.end());
        sort(grains.begin(), grains.end());
        int l = 0;
        int r = abs(hens[0] - grains[0]) + grains[m - 1] - grains[0];
        auto check = [&](int t) -> bool {
            int j = 0;
            for (int x : hens) {
                if (j == m) {
                    return true;
                }
                int y = grains[j];
                if (y <= x) {
                    int d = x - y;
                    if (d > t) {
                        return false;
                    }
                    while (j < m && grains[j] <= x) {
                        ++j;
                    }
                    while (j < m && min(d, grains[j] - x) + grains[j] - y <= t) {
                        ++j;
                    }
                } else {
                    while (j < m && grains[j] - x <= t) {
                        ++j;
                    }
                }
            }
            return j == m;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->