# Sort Colors
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

 

Example 1:

Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
Example 2:

Input: nums = [2,0,1]
Output: [0,1,2]
 

Constraints:

n == nums.length
1 <= n <= 300
nums[i] is either 0, 1, or 2.

# Solution

```JavaScript
/**
 * @param {number[]} nums
 * @return {void}  // in-place
 */
var sortColors = function (nums) {
  let low = 0;              // next spot for 0
  let mid = 0;              // current index
  let high = nums.length-1; // next spot for 2

  while (mid <= high) {
    switch (nums[mid]) {
      case 0:               // red → swap to front
        [nums[low], nums[mid]] = [nums[mid], nums[low]];
        low++;
        mid++;
        break;

      case 1:               // white → already in right band
        mid++;
        break;

      case 2:               // blue → swap to end
        [nums[mid], nums[high]] = [nums[high], nums[mid]];
        high--;
        break;
    }
  }
};

 
