---
title: Letter Combinations of a Phone Number
date: 2019-07-06 18:22:09
tags: Array
categories: Algorithm  
---

## Q:
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

#### Example:
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.

## A:
``` js
/**
 * @param {string} digits
 * @return {string[]}
 */
var letterCombinations = function(digits) {
    var tmp = [" ", " ", "abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"];
    var ans = [];
    var digitsArr = digits.split('');
    var tmpArr = [];
    
    for(item in digitsArr) {
        tmpArr.push(tmp[digitsArr[item]])
    }

    if (tmpArr.length == 1) {
        ans = tmpArr[0].split('');
        return ans;
    } else if (tmpArr.length < 1) {
        return []; 
    }

    ans = tmpArr[0];
    for (var i = 0; i < tmpArr.length - 1; i++) {
        ans = match(ans, tmpArr[i+1]);
    }

    function match(str1, str2) {
        var group = [];
        if(typeof str1 == 'string') {
            arr1 = str1.split('');
        } else {
            arr1 = str1;
        }

        arr2 = str2.split(''); 
  
        for (var m = 0; m < arr1.length; m++) {
            for (var n = 0; n < arr2.length; n++) {
                group.push(arr1[m] + arr2[n]);          
            }
        }

        return group;
    }

    return ans;
};
```