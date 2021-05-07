---
title: Valid Parentheses
date: 2019-09-01 21:58:09
tags: regex
categories: Algorithm  
---

## Q:
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

#### Example 1:
``` js
Input: "()"
Output: true
```

#### Example 2:
``` js
Input: "()[]{}"
Output: true
```

#### Example 3:
``` js
Input: "(]"
Output: false
```

#### Example 4:
``` js
Input: "([)]"
Output: false
```

#### Example 5:
``` js
Input: "{[]}"
Output: true
``` 

## A:
``` js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    var sl = s.length;
    while (sl > 0) {
        s = s.replace('{}', '')
        s = s.replace('[]', '')
        s = s.replace('()', '')

        sl--;
    }
    
    return s == '';
};
```
 
## Appendix:
## Tricks about regex:
1. Most common flags : 
  - i : case insensitive 
  - g : global (doesn't stop after first match)
  - m : multi-line
2. Most common anchors : 
  - ^ : Start of string
  - $ : End of string
  - \A : Start of string (not affected by multi-line content)
  - \Z : End of string (not affected by multi-line content)
3. Most common quantifiers : 
  - {n} : Exactly n times
  - {n,} : At least n times
  - {m,n} : Between m and n times
  - ? : Zero or one time
  - \+ : One or more times
  - \* : Zero, one or more times
4. Most common meta sequences : 
  - . : Any character but \n and \r 
  - \w | \W : Any word character | Any non-word character
  - \d | \D : Any digit character | Any non-digit character
  - \s | \S : Any whitespace character | Any non-whitespace character
5. Character set : 
  - [abc] : Will match either a, b or c
  - [1-9] : Will match any digit from 1 to 9
  - [a-zA-Z] : Will match any letter
6. Match any character but : 
  - [^abc] : Matches anything but a, b or c
7. Escape a character : 
  - \character (example : escaping + => \+)
8. Refer to a group (also used for capturing groups, look further): 
  - (group of characters) (example : /(he)+/ will match 'hehehe'
9. One group or another : 
  - | : /^h((ello)|(ola))$/ will match both 'hello' and 'hola'
