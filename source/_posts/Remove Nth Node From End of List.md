---
title: Remove Nth Node From End of List
date: 2019-08-25 21:35:09
tags: 
  - List
  - 双指针
categories: Algorithm  
---
## Remove Nth Node From End of List
Given a linked list, remove the n-th node from the end of list and return its head.

## Example:
> Given linked list: 1->2->3->4->5, and n = 2.
  After removing the second node from the end, the linked list becomes 1->2->3->5.
  
## Note:
Given n will always be valid.

## Ans:
``` js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {
    var temp = head;
    for (let i = 0; i < n; i++) {
        temp = temp.next;
    }
    
    if(!temp) return head.next; // n equals list.length
    
    var cur = head;
    while(temp.next) {
        temp = temp.next;
        cur = cur.next;
    }
    
    cur.next = cur.next.next;
    
    return head;
};
```
 

又是令人头秃的一天~