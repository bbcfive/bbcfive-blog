---
title: 3Sum
date: 2019-06-28 08:29:08
tags: Array
categories: Algorithm 
---

## Q：

Given an array nums of n integers, are there elements a , b , c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

#### Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
[-1, 0, 1],
[-1, -1, 2]
] 
 
## A：
``` js
/**
 * 3Sum
 * 三数之和
*/

// Complexity: O(n^2)

/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  nums.sort((a, b) => a - b)

  let ans = []
  let len = nums.length

  // enumerate the array, and assume the item to be the smallest one
  for (let i = 0; i < len; i++ ) { 

    // have already enumerate the item as the smallest one among the three
    // then continue
    if (i && nums[i] === nums[i - 1]) continue 

    // the sum of another two should be
    let target = -nums[i]

    // the indexes of another two 
    let [start, end] = [i + 1, len - 1]

    while (start < end) {
      let sum = nums[start] + nums[end]

      if (sum > target) {
        end--
      } else if (sum < target) {
        start++
      } else {
        ans.push([nums[i], nums[start], nums[end]])
        
        // remove the duplication
        while (nums[start] === nums[start + 1]) 
          start++
        start++

        // remove the duplication
        while (nums[end] === nums[end - 1])
          end--
        end--
      }
    }
  }

  return ans
}
```