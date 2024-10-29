# [2605. Form Smallest Number From Two Digit Arrays](https://leetcode.com/problems/form-smallest-number-from-two-digit-arrays)


## Description

<!-- description:start -->

Given two arrays of <strong>unique</strong> digits <code>nums1</code> and <code>nums2</code>, return <em>the <strong>smallest</strong> number that contains <strong>at least</strong> one digit from each array</em>.

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [4,1,3], nums2 = [5,7]
<strong>Output:</strong> 15
<strong>Explanation:</strong> The number 15 contains the digit 1 from nums1 and the digit 5 from nums2. It can be proven that 15 is the smallest number we can have.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [3,5,2,6], nums2 = [3,1,7]
<strong>Output:</strong> 3
<strong>Explanation:</strong> The number 3 contains the digit 3 which exists in both arrays.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 9</code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 9</code></li>
	<li>All digits in each array are <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We observe that if there are the same numbers in the arrays $nums1$ and $nums2$, then the minimum of the same numbers is the smallest number. Otherwise, we take the number $a$ in the array $nums1$ and the number $b$ in the array $nums2$, and concatenate the two numbers $a$ and $b$ into two numbers, and take the smaller number.

The time complexity is $O(m \times n)$, and the space complexity is $O(1)$, where $m$ and $n$ are the lengths of the arrays $nums1$ and $nums2$.


#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        int ans = 100;
        for (int a : nums1) {
            for (int b : nums2) {
                if (a == b) {
                    ans = Math.min(ans, a);
                } else {
                    ans = Math.min(ans, Math.min(a * 10 + b, b * 10 + a));
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
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        int ans = 100;
        for (int a : nums1) {
            for (int b : nums2) {
                if (a == b) {
                    ans = min(ans, a);
                } else {
                    ans = min({ans, a * 10 + b, b * 10 + a});
                }
            }
        }
        return ans;
    }
};
```


### Solution 2: Hash Table or Array + Enumeration

We can use a hash table or array to record the numbers in the arrays $nums1$ and $nums2$, and then enumerate $1 \sim 9$. If $i$ appears in both arrays, then $i$ is the smallest number. Otherwise, we take the number $a$ in the array $nums1$ and the number $b$ in the array $nums2$, and concatenate the two numbers $a$ and $b$ into two numbers, and take the smaller number.

The time complexity is $(m + n)$, and the space complexity is $O(C)$. Where $m$ and $n$ are the lengths of the arrays $nums1$ and $nums2$ respectively; and $C$ is the range of the numbers in the arrays $nums1$ and $nums2$, and the range in this problem is $C = 10$.


#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        boolean[] s1 = new boolean[10];
        boolean[] s2 = new boolean[10];
        for (int x : nums1) {
            s1[x] = true;
        }
        for (int x : nums2) {
            s2[x] = true;
        }
        int a = 0, b = 0;
        for (int i = 1; i < 10; ++i) {
            if (s1[i] && s2[i]) {
                return i;
            }
            if (a == 0 && s1[i]) {
                a = i;
            }
            if (b == 0 && s2[i]) {
                b = i;
            }
        }
        return Math.min(a * 10 + b, b * 10 + a);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        bitset<10> s1;
        bitset<10> s2;
        for (int x : nums1) {
            s1[x] = 1;
        }
        for (int x : nums2) {
            s2[x] = 1;
        }
        int a = 0, b = 0;
        for (int i = 1; i < 10; ++i) {
            if (s1[i] && s2[i]) {
                return i;
            }
            if (!a && s1[i]) {
                a = i;
            }
            if (!b && s2[i]) {
                b = i;
            }
        }
        return min(a * 10 + b, b * 10 + a);
    }
};
```


<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Bit Operation

Since the range of the numbers is $1 \sim 9$, we can use a binary number with a length of $10$ to represent the numbers in the arrays $nums1$ and $nums2$. We use $mask1$ to represent the numbers in the array $nums1$, and use $mask2$ to represent the numbers in the array $nums2$.

If the number $mask$ obtained by performing a bitwise AND operation on $mask1$ and $mask2$ is not equal to $0$, then we extract the position of the last $1$ in the number $mask$, which is the smallest number.

Otherwise, we extract the position of the last $1$ in $mask1$ and $mask2$ respectively, and denote them as $a$ and $b$, respectively. Then the smallest number is $min(a \times 10 + b, b \times 10 + a)$.

The time complexity is $O(m + n)$, and the space complexity is $O(1)$. Where $m$ and $n$ are the lengths of the arrays $nums1$ and $nums2$ respectively.


#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        int mask1 = 0, mask2 = 0;
        for (int x : nums1) {
            mask1 |= 1 << x;
        }
        for (int x : nums2) {
            mask2 |= 1 << x;
        }
        int mask = mask1 & mask2;
        if (mask != 0) {
            return Integer.numberOfTrailingZeros(mask);
        }
        int a = Integer.numberOfTrailingZeros(mask1);
        int b = Integer.numberOfTrailingZeros(mask2);
        return Math.min(a * 10 + b, b * 10 + a);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        int mask1 = 0, mask2 = 0;
        for (int x : nums1) {
            mask1 |= 1 << x;
        }
        for (int x : nums2) {
            mask2 |= 1 << x;
        }
        int mask = mask1 & mask2;
        if (mask) {
            return __builtin_ctz(mask);
        }
        int a = __builtin_ctz(mask1);
        int b = __builtin_ctz(mask2);
        return min(a * 10 + b, b * 10 + a);
    }
};
```

<!-- solution:end -->

<!-- problem:end -->