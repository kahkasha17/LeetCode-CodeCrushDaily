# [137. Single Number II](https://leetcode.com/problems/single-number-ii)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> where&nbsp;every element appears <strong>three times</strong> except for one, which appears <strong>exactly once</strong>. <em>Find the single element and return it</em>.</p>

<p>You must&nbsp;implement a solution with a linear runtime complexity and use&nbsp;only constant&nbsp;extra space.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [2,2,3,2]
<strong>Output:</strong> 3
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [0,1,0,1,0,1,99]
<strong>Output:</strong> 99
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li>Each element in <code>nums</code> appears exactly <strong>three times</strong> except for one element which appears <strong>once</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Bitwise Operation

We can enumerate each binary bit $i$, and for each binary bit, we calculate the sum of all numbers on that bit. If the sum of the numbers on that bit can be divided by 3, then the number that only appears once on that bit is 0, otherwise it is 1.

The time complexity is $O(n \times \log M)$, where $n$ and $M$ are the length of the array and the range of elements in the array, respectively. The space complexity is $O(1)$.

#### Java

```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int cnt = 0;
            for (int num : nums) {
                cnt += num >> i & 1;
            }
            cnt %= 3;
            ans |= cnt << i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int cnt = 0;
            for (int num : nums) {
                cnt += ((num >> i) & 1);
            }
            cnt %= 3;
            ans |= cnt << i;
        }
        return ans;
    }
};
```
#### JavaScript

```js
function singleNumber(nums) {
    let ans = 0;
    for (let i = 0; i < 32; i++) {
        const count = nums.reduce((r, v) => r + ((v >> i) & 1), 0);
        ans |= count % 3 << i;
    }
    return ans;
}
```
#### C

```c
int singleNumber(int* nums, int numsSize) {
    int ans = 0;
    for (int i = 0; i < 32; i++) {
        int count = 0;
        for (int j = 0; j < numsSize; j++) {
            if (nums[j] >> i & 1) {
                count++;
            }
        }
        ans |= (uint) (count % 3) << i;
    }
    return ans;
}
```
### Solution 2: Digital Circuit

We can use a more efficient method that uses digital circuits to simulate the above bitwise operation.

Each binary bit of an integer can only represent 2 states, 0 or 1. However, we need to represent the sum of the $i$-th bit of all integers traversed so far modulo 3. Therefore, we can use two integers $a$ and $b$ to represent it. There are three possible cases:

1. The $i$-th bit of integer $a$ is 0 and the $i$-th bit of integer $b$ is 0, which means the modulo 3 result is 0;
2. The $i$-th bit of integer $a$ is 0 and the $i$-th bit of integer $b$ is 1, which means the modulo 3 result is 1;
3. The $i$-th bit of integer $a$ is 1 and the $i$-th bit of integer $b$ is 0, which means the modulo 3 result is 2.

We use integer $c$ to represent the number to be read in, and the truth table is as follows:

| $a_i$ | $b_i$ | $c_i$ | New $a_i$ | New $b_i$ |
| ----- | ----- | ----- | --------- | --------- |
| 0     | 0     | 0     | 0         | 0         |
| 0     | 0     | 1     | 0         | 1         |
| 0     | 1     | 0     | 0         | 1         |
| 0     | 1     | 1     | 1         | 0         |
| 1     | 0     | 0     | 1         | 0         |
| 1     | 0     | 1     | 0         | 0         |

Based on the truth table, we can write the logical expression:

$$
a_i = a_i' b_i c_i + a_i b_i' c_i'
$$

and:

$$
b_i = a_i' b_i' c_i + a_i' b_i c_i' = a_i' (b_i \oplus c_i)
$$

The final result is $b$, because when the binary bit of $b$ is 1, it means that the number appears only once.

The time complexity is $O(n)$, where $n$ is the length of the array. The space complexity is $O(1)$.

#### Java

```java
class Solution {
    public int singleNumber(int[] nums) {
        int a = 0, b = 0;
        for (int c : nums) {
            int aa = (~a & b & c) | (a & ~b & ~c);
            int bb = ~a & (b ^ c);
            a = aa;
            b = bb;
        }
        return b;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int a = 0, b = 0;
        for (int c : nums) {
            int aa = (~a & b & c) | (a & ~b & ~c);
            int bb = ~a & (b ^ c);
            a = aa;
            b = bb;
        }
        return b;
    }
};
```
#### JavaScript

```js
function singleNumber(nums) {
    let a = 0;
    let b = 0;
    for (const c of nums) {
        const aa = (~a & b & c) | (a & ~b & ~c);
        const bb = ~a & (b ^ c);
        a = aa;
        b = bb;
    }
    return b;
}
```
### Solution 3: Set + Math

#### JavaScript

```js
function singleNumber(nums) {
    const sumOfUnique = [...new Set(nums)].reduce((a, b) => a + b, 0);
    const sum = nums.reduce((a, b) => a + b, 0);
    return (sumOfUnique * 3 - sum) / 2;
}
```
### Solution 4: Bit Manipulation

#### JavaScript

```ts
function singleNumber(nums) {
    let [ans, acc] = [0, 0];

    for (const x of nums) {
        ans ^= x & ~acc;
        acc ^= x & ~ans;
    }

    return ans;
}
```


<!-- solution:end -->

<!-- problem:end -->