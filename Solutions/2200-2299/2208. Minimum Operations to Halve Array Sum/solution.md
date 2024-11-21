# [2208. Minimum Operations to Halve Array Sum](https://leetcode.com/problems/minimum-operations-to-halve-array-sum)

## Description

<!-- description:start -->

<p>You are given an array <code>nums</code> of positive integers. In one operation, you can choose <strong>any</strong> number from <code>nums</code> and reduce it to <strong>exactly</strong> half the number. (Note that you may choose this reduced number in future operations.)</p>

<p>Return<em> the <strong>minimum</strong> number of operations to reduce the sum of </em><code>nums</code><em> by <strong>at least</strong> half.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,19,8,1]
<strong>Output:</strong> 3
<strong>Explanation:</strong> The initial sum of nums is equal to 5 + 19 + 8 + 1 = 33.
The following is one of the ways to reduce the sum by at least half:
Pick the number 19 and reduce it to 9.5.
Pick the number 9.5 and reduce it to 4.75.
Pick the number 8 and reduce it to 4.
The final array is [5, 4.75, 4, 1] with a total sum of 5 + 4.75 + 4 + 1 = 14.75. 
The sum of nums has been reduced by 33 - 14.75 = 18.25, which is at least half of the initial sum, 18.25 &gt;= 33/2 = 16.5.
Overall, 3 operations were used so we return 3.
It can be shown that we cannot reduce the sum by at least half in less than 3 operations.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,8,20]
<strong>Output:</strong> 3
<strong>Explanation:</strong> The initial sum of nums is equal to 3 + 8 + 20 = 31.
The following is one of the ways to reduce the sum by at least half:
Pick the number 20 and reduce it to 10.
Pick the number 10 and reduce it to 5.
Pick the number 3 and reduce it to 1.5.
The final array is [1.5, 8, 5] with a total sum of 1.5 + 8 + 5 = 14.5. 
The sum of nums has been reduced by 31 - 14.5 = 16.5, which is at least half of the initial sum, 16.5 &gt;= 31/2 = 15.5.
Overall, 3 operations were used so we return 3.
It can be shown that we cannot reduce the sum by at least half in less than 3 operations.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Greedy + Priority Queue (Max Heap)

According to the problem description, each operation will halve a number in the array. To minimize the number of operations that reduce the array sum by at least half, each operation should halve the current maximum value in the array.

Therefore, we first calculate the total sum $s$ that the array needs to reduce, and then use a priority queue (max heap) to maintain all the numbers in the array. Each time we take the maximum value $t$ from the priority queue, halve it, and then put the halved number back into the priority queue, while updating $s$, until $s \le 0$. The number of operations at this time is the answer.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array.

#### Java

```java
class Solution {
    public int halveArray(int[] nums) {
        PriorityQueue<Double> pq = new PriorityQueue<>(Collections.reverseOrder());
        double s = 0;
        for (int x : nums) {
            s += x;
            pq.offer((double) x);
        }
        s /= 2.0;
        int ans = 0;
        while (s > 0) {
            double t = pq.poll() / 2.0;
            s -= t;
            pq.offer(t);
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int halveArray(vector<int>& nums) {
        priority_queue<double> pq;
        double s = 0;
        for (int x : nums) {
            s += x;
            pq.push((double) x);
        }
        s /= 2.0;
        int ans = 0;
        while (s > 0) {
            double t = pq.top() / 2.0;
            pq.pop();
            s -= t;
            pq.push(t);
            ++ans;
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->