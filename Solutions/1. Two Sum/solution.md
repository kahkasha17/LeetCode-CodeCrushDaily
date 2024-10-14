<h1>TWO SUM</h1>

<p>Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.</p>
<p>You may assume that each input would have exactly one solution, and you may not use the same element twice.</p>
<p>You can return the answer in any order.</p><br>
<b>Example 1:</b><br>
<p><b>Input: </b>nums = [2,7,11,15], target = 9</p>
<p><b>Output: </b>[0,1]</p>
<p><b>Explanation: </b>Because nums[0] + nums[1] == 9, we return [0, 1].</p>
<br>
<b>Example 2:</b><br>
<p><b>Input: </b>nums = [3,2,4], target = 6</p>
<p><b>Output: </b>[1,2]</p>
<br>
<b>Example 3:</b><br>
<p><b>Input: </b>nums = [3,3], target = 6</p>
<p><b>Output: </b>[0,1]</p>
<br><br>
<h3>Constraints:</h3>
<ul>
  <li>2 <= nums.length <= 104</li>
<li>-109 <= nums[i] <= 109</li>
<li>-109 <= target <= 109</li>
<li><b>Only one valid answer exists.<b></li>
</ul>
<br>
<p><b>Follow-up: </b>Can you come up with an algorithm that is less than O(n2) time complexity?
</p>
<br><br>

<h1>Solution:</h1>
<h3>JavaScript</h3>

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let hashMap = new Map();

    for(let i = 0; i < nums.length; i++) {
        let neededNumber = target - nums[i];

        if(hashMap.has(neededNumber)) {
            return [i, hashMap.get(neededNumber)];
        } 
        hashMap.set(nums[i], i);

    }
};
```
<h3>Java</h3>

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        
        Map<Integer, Integer> sumMap = new HashMap<>();
        int [] result = new int[2];
        
        for (int i=0; i<nums.length(); i++){
            
            if(!sumMap.isEmpty() && sumMap.containsKey(nums[i])){
                result[0] = sumMap.get(nums[i]);
                result[1] = i;
                
                return result;
            }
            else{
                sumMap.put(target-nums[i],i);
            }
        }
    }
}
```
