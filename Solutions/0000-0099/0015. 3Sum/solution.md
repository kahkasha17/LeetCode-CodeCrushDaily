
# [15. 3Sum](https://leetcode.com/problems/3sum)


## Description


<p>Given an integer array nums, return all the triplets <code>[nums[i], nums[j], nums[k]]</code> such that <code>i != j</code>, <code>i != k</code>, and <code>j != k</code>, and <code>nums[i] + nums[j] + nums[k] == 0</code>.</p>

<p>Notice that the solution set must not contain duplicate triplets.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [-1,0,1,2,-1,-4]
<strong>Output:</strong> [[-1,-1,2],[-1,0,1]]
<strong>Explanation:</strong> 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,1,1]
<strong>Output:</strong> []
<strong>Explanation:</strong> The only possible triplet does not sum up to 0.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,0,0]
<strong>Output:</strong> [[0,0,0]]
<strong>Explanation:</strong> The only possible triplet sums up to 0.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 3000</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>


## Solutions


### Solution 1: Sort + Two Pointers

We notice that the problem does not require us to return the triplet in order, so we might as well sort the array first, which makes it easy to skip duplicate elements.

Next, we enumerate the first element of the triplet $nums[i]$, where $0 \leq i \lt n - 2$. For each $i$, we can find $j$ and $k$ satisfying $nums[i] + nums[j] + nums[k] = 0$ by maintaining two pointers $j = i + 1$ and $k = n - 1$. In the enumeration process, we need to skip duplicate elements to avoid duplicate triplets.

The specific judgment logic is as follows:

If $i \gt 0$ and $nums[i] = nums[i - 1]$, it means that the element currently enumerated is the same as the previous element, we can skip it directly, because it will not produce new results.

If $nums[i] \gt 0$, it means that the element currently enumerated is greater than $0$, so the sum of three numbers must not be equal to $0$, and the enumeration ends.

Otherwise, we let the left pointer $j = i + 1$, and the right pointer $k = n - 1$. When $j \lt k$, the loop is executed, and the sum of three numbers $x = nums[i] + nums[j] + nums[k]$ is calculated and compared with $0$:

-   If $x \lt 0$, it means that $nums[j]$ is too small, we need to move $j$ to the right.
-   If $x \gt 0$, it means that $nums[k]$ is too large, we need to move $k$ to the left.
-   Otherwise, it means that we have found a valid triplet, add it to the answer, move $j$ to the right, move $k$ to the left, and skip all duplicate elements to continue looking for the next valid triplet.

After the enumeration is over, we can get the answer to the triplet.

The time complexity is $O(n^2)$, and the space complexity is $O(\log n)$. The $n$ is the length of the array.



#### Java

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < n - 2 && nums[i] <= 0; ++i) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int j = i + 1, k = n - 1;
            while (j < k) {
                int x = nums[i] + nums[j] + nums[k];
                if (x < 0) {
                    ++j;
                } else if (x > 0) {
                    --k;
                } else {
                    ans.add(List.of(nums[i], nums[j++], nums[k--]));
                    while (j < k && nums[j] == nums[j - 1]) {
                        ++j;
                    }
                    while (j < k && nums[k] == nums[k + 1]) {
                        --k;
                    }
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
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int n = nums.size();
        for (int i = 0; i < n - 2 && nums[i] <= 0; ++i) {
            if (i && nums[i] == nums[i - 1]) {
                continue;
            }
            int j = i + 1, k = n - 1;
            while (j < k) {
                int x = nums[i] + nums[j] + nums[k];
                if (x < 0) {
                    ++j;
                } else if (x > 0) {
                    --k;
                } else {
                    ans.push_back({nums[i], nums[j++], nums[k--]});
                    while (j < k && nums[j] == nums[j - 1]) {
                        ++j;
                    }
                    while (j < k && nums[k] == nums[k + 1]) {
                        --k;
                    }
                }
            }
        }
        return ans;
    }
};
```


#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function (nums) {
    const n = nums.length;
    nums.sort((a, b) => a - b);
    const ans = [];
    for (let i = 0; i < n - 2 && nums[i] <= 0; ++i) {
        if (i > 0 && nums[i] === nums[i - 1]) {
            continue;
        }
        let j = i + 1;
        let k = n - 1;
        while (j < k) {
            const x = nums[i] + nums[j] + nums[k];
            if (x < 0) {
                ++j;
            } else if (x > 0) {
                --k;
            } else {
                ans.push([nums[i], nums[j++], nums[k--]]);
                while (j < k && nums[j] === nums[j - 1]) {
                    ++j;
                }
                while (j < k && nums[k] === nums[k + 1]) {
                    --k;
                }
            }
        }
    }
    return ans;
};
```

