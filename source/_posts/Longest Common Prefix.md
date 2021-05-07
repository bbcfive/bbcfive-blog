---
title: Longest Common Prefix
date: 2019-06-21 21:11:08
tags: String
categories: Algorithm 
---

## Q：

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

#### Example 1:

Input: ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

Note:
All given inputs are in lowercase letters a-z.

## A:
``` js
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function(strs) {
    if (!strs.length) return '';
    if (strs.length == 1) return strs[0];
    
    var minest = 0;
    for (var i = 0; i < strs.length - 1; i++) {
        minest = Math.min(strs[i].length, strs[i+1].length);
    }

    var index = 0, longest = 0;
    while(index < minest) {
        for (var i = 0; i < strs.length - 1; i++) {

            if (strs[i][index] !== strs[i+1][index]) {
                longest = index;

                i = strs.length - 1
                index = minest
            } else {
                longest = index + 1;
            }
        }
        index++;
    }

    return strs[0].slice(0,longest);  
};
```