
# [4. Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays)


## Description


<p>Given two sorted arrays <code>nums1</code> and <code>nums2</code> of size <code>m</code> and <code>n</code> respectively, return <strong>the median</strong> of the two sorted arrays.</p>

<p>The overall run time complexity should be <code>O(log (m+n))</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,3], nums2 = [2]
<strong>Output:</strong> 2.00000
<strong>Explanation:</strong> merged array = [1,2,3] and median is 2.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,2], nums2 = [3,4]
<strong>Output:</strong> 2.50000
<strong>Explanation:</strong> merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>nums1.length == m</code></li>
	<li><code>nums2.length == n</code></li>
	<li><code>0 &lt;= m &lt;= 1000</code></li>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= m + n &lt;= 2000</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
</ul>


## Solutions


### Solution 1: Divide and Conquer

The problem requires the time complexity of the algorithm to be $O(\log (m + n))$, so we cannot directly traverse the two arrays, but need to use the binary search method.

If $m + n$ is odd, then the median is the $\left\lfloor\frac{m + n + 1}{2}\right\rfloor$-th number; if $m + n$ is even, then the median is the average of the $\left\lfloor\frac{m + n + 1}{2}\right\rfloor$-th and the $\left\lfloor\frac{m + n + 2}{2}\right\rfloor$-th numbers. In fact, we can unify it as the average of the $\left\lfloor\frac{m + n + 1}{2}\right\rfloor$-th and the $\left\lfloor\frac{m + n + 2}{2}\right\rfloor$-th numbers.

Therefore, we can design a function $f(i, j, k)$, which represents the $k$-th smallest number in the interval $[i, m)$ of array $nums1$ and the interval $[j, n)$ of array $nums2$. The median is the average of $f(0, 0, \left\lfloor\frac{m + n + 1}{2}\right\rfloor)$ and $f(0, 0, \left\lfloor\frac{m + n + 2}{2}\right\rfloor)$.

The implementation idea of the function $f(i, j, k)$ is as follows:

-   If $i \geq m$, it means that the interval $[i, m)$ of array $nums1$ is empty, so directly return $nums2[j + k - 1]$;
-   If $j \geq n$, it means that the interval $[j, n)$ of array $nums2$ is empty, so directly return $nums1[i + k - 1]$;
-   If $k = 1$, it means to find the first number, so just return the minimum of $nums1[i]$ and $nums2[j]$;
-   Otherwise, we find the $\left\lfloor\frac{k}{2}\right\rfloor$-th number in the two arrays, denoted as $x$ and $y$. (Note, if a certain array does not have the $\left\lfloor\frac{k}{2}\right\rfloor$-th number, then we regard the $\left\lfloor\frac{k}{2}\right\rfloor$-th number as $+\infty$.) Compare the size of $x$ and $y$:
    -   If $x \leq y$, it means that the $\left\lfloor\frac{k}{2}\right\rfloor$-th number of array $nums1$ cannot be the $k$-th smallest number, so we can exclude the interval $[i, i + \left\lfloor\frac{k}{2}\right\rfloor)$ of array $nums1$, and recursively call $f(i + \left\lfloor\frac{k}{2}\right\rfloor, j, k - \left\lfloor\frac{k}{2}\right\rfloor)$.
    -   If $x > y$, it means that the $\left\lfloor\frac{k}{2}\right\rfloor$-th number of array $nums2$ cannot be the $k$-th smallest number, so we can exclude the interval $[j, j + \left\lfloor\frac{k}{2}\right\rfloor)$ of array $nums2$, and recursively call $f(i, j + \left\lfloor\frac{k}{2}\right\rfloor, k - \left\lfloor\frac{k}{2}\right\rfloor)$.

The time complexity is $O(\log(m + n))$, and the space complexity is $O(\log(m + n))$. Here, $m$ and $n$ are the lengths of arrays $nums1$ and $nums2$ respectively.


#### Java

```java
class Solution {
    private int m;
    private int n;
    private int[] nums1;
    private int[] nums2;

    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        m = nums1.length;
        n = nums2.length;
        this.nums1 = nums1;
        this.nums2 = nums2;
        int a = f(0, 0, (m + n + 1) / 2);
        int b = f(0, 0, (m + n + 2) / 2);
        return (a + b) / 2.0;
    }

    private int f(int i, int j, int k) {
        if (i >= m) {
            return nums2[j + k - 1];
        }
        if (j >= n) {
            return nums1[i + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[i], nums2[j]);
        }
        int p = k / 2;
        int x = i + p - 1 < m ? nums1[i + p - 1] : 1 << 30;
        int y = j + p - 1 < n ? nums2[j + p - 1] : 1 << 30;
        return x < y ? f(i + p, j, k - p) : f(i, j + p, k - p);
    }
}
```

#### C++

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        function<int(int, int, int)> f = [&](int i, int j, int k) {
            if (i >= m) {
                return nums2[j + k - 1];
            }
            if (j >= n) {
                return nums1[i + k - 1];
            }
            if (k == 1) {
                return min(nums1[i], nums2[j]);
            }
            int p = k / 2;
            int x = i + p - 1 < m ? nums1[i + p - 1] : 1 << 30;
            int y = j + p - 1 < n ? nums2[j + p - 1] : 1 << 30;
            return x < y ? f(i + p, j, k - p) : f(i, j + p, k - p);
        };
        int a = f(0, 0, (m + n + 1) / 2);
        int b = f(0, 0, (m + n + 2) / 2);
        return (a + b) / 2.0;
    }
};
```


#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number}
 */
var findMedianSortedArrays = function (nums1, nums2) {
    const m = nums1.length;
    const n = nums2.length;
    const f = (i, j, k) => {
        if (i >= m) {
            return nums2[j + k - 1];
        }
        if (j >= n) {
            return nums1[i + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[i], nums2[j]);
        }
        const p = Math.floor(k / 2);
        const x = i + p - 1 < m ? nums1[i + p - 1] : 1 << 30;
        const y = j + p - 1 < n ? nums2[j + p - 1] : 1 << 30;
        return x < y ? f(i + p, j, k - p) : f(i, j + p, k - p);
    };
    const a = f(0, 0, Math.floor((m + n + 1) / 2));
    const b = f(0, 0, Math.floor((m + n + 2) / 2));
    return (a + b) / 2;
};
```

