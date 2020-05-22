---
title: 3Sum Closest
date: 2019-06-28 08:32:08
tags: Array
categories: Algorithm 
---
## Q：

Given an array nums of n integers and an integer target , find three integers in nums such that the sum is closest to target . Return the sum of the three integers. You may assume that each input would have exactly one solution.

Example:

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2). 
 
## A：
``` js
/**
 * 3Sum Closest
 * 最接近的三数之和
*/

/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */

function binarySearch(a, target) {
  var start = 0
    , end = a.length - 1;

  while(start <= end) {
    var mid = ~~((start + end) >> 1);
    if (a[mid] >= target)
      end = mid - 1;
    else 
      start = mid + 1;
  }

  return start;
}


var threeSumClosest = function(nums, target) {
  nums.sort(function(a, b) {
    return a - b;
  });

  var len = nums.length;
  var ans = Infinity;

  for (var i = 0; i < len; i++)
    for (var j = i + 1; j < len; j++) {
      var a = target - nums[i] - nums[j];
      var pos = binarySearch(nums, a);
      
      for (var k = Math.max(0, pos - 1); k <= Math.min(pos + 0, len - 1); k++) {
        if (k === i || k === j) 
          continue;

        var sum = nums[i] + nums[j] + nums[k];
        if (Math.abs(sum - target) < Math.abs(ans - target))
          ans = sum;
      }

    }

  return ans;
};
```