# Problem
Continuous Subarrays

You are given a 0-indexed integer array nums. A subarray of nums is called continuous if:

Let i, i + 1, ..., j be the indices in the subarray. Then, for each pair of indices i <= i1, i2 <= j, 0 <= |nums[i1] - nums[i2]| <= 2.

Return the total number of continuous subarrays.

A subarray is a contiguous non-empty sequence of elements within an array.

 

Example 1:

Input: nums = [5,4,2,4]

Output: 8

Explanation: 

Continuous subarray of size 1: [5], [4], [2], [4].

Continuous subarray of size 2: [5,4], [4,2], [2,4].

Continuous subarray of size 3: [4,2,4].

There are no subarrys of size 4.

Total continuous subarrays = 4 + 3 + 1 = 8.

It can be shown that there are no more continuous subarrays.
 

Example 2:

Input: nums = [1,2,3]

Output: 6

Explanation: 

Continuous subarray of size 1: [1], [2], [3].

Continuous subarray of size 2: [1,2], [2,3].

Continuous subarray of size 3: [1,2,3].

Total continuous subarrays = 3 + 2 + 1 = 6.
 

Constraints:

1 <= nums.length <= 105

1 <= nums[i] <= 109

# Solution

```Javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var continuousSubarrays = function(nums) {
    let left = 0, result = 0;
    const maxDeque = [], minDeque = [];

    for (let right = 0; right < nums.length; right++) {
        // Maintain decreasing maxDeque
        while (maxDeque.length && nums[right] > maxDeque[maxDeque.length - 1]) {
            maxDeque.pop();
        }
        maxDeque.push(nums[right]);

        // Maintain increasing minDeque
        while (minDeque.length && nums[right] < minDeque[minDeque.length - 1]) {
            minDeque.pop();
        }
        minDeque.push(nums[right]);

        // Shrink window if the max - min > 2
        while (maxDeque[0] - minDeque[0] > 2) {
            if (maxDeque[0] === nums[left]) maxDeque.shift();
            if (minDeque[0] === nums[left]) minDeque.shift();
            left++;
        }

        // Count subarrays ending at 'right'
        result += (right - left + 1);
    }

    return result;
};
