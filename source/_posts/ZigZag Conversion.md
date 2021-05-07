---
title: ZigZag Conversion
date: 2019-05-13 09:15:09
tags: Array
categories: Algorithm  
---


## 一、问题

将字符串 "PAYPALISHIRING" 以Z字形排列成给定的行数：

P A H N
A P L S I I G
Y I R

之后从左往右，逐行读取字符： "PAHNAPLSIIGYIR"

实现一个将字符串进行指定行数变换的函数:

string convert(string s, int numRows);

示例 1:

输入: s = "PAYPALISHIRING", numRows = 3
输出: "PAHNAPLSIIGYIR"

示例 2:

输入: s = "PAYPALISHIRING", numRows = 4
输出: "PINALSIGYAHRPI"
解释:

P I N
A L S I G
Y A H R
P I

## 二、解决

JAVA
``` java
class Solution {
    public String convert(String s, int numRows) {

        if (numRows == 1) return s;

        List<StringBuilder> rows = new ArrayList<>();
        for (int i = 0; i < Math.min(numRows, s.length()); i++)
            rows.add(new StringBuilder());

        int curRow = 0;
        boolean goingDown = false;

        for (char c : s.toCharArray()) {
            rows.get(curRow).append(c);
            if (curRow == 0 || curRow == numRows - 1) goingDown = !goingDown;
            curRow += goingDown ? 1 : -1;
        }

        StringBuilder ret = new StringBuilder();
        for (StringBuilder row : rows) ret.append(row);
        return ret.toString();
    }
}
```
 
JAVASCRIPT
``` js
var convert = function(s, numRows) {
    var strArr = s.split('');
    var arr = new Array(numRows);
    for (var i = 0; i < numRows; i++) {
        arr[i] = [];
    }

    if (numRows == 1) {
        return s;
    }

    var zig = 0, zag = numRows - 2;
    while (strArr.length > 0) {
        if (zig < numRows) {
            arr[zig].push(strArr.shift());
            zig++;
            zag = numRows - 2;
        } else if (zag > 0) {
            arr[zag].push(strArr.shift());
            zag--;                 
        }
        if (zag == 0 && zig == numRows) {
            zig = 0;
        }             
    }            

    var result = '';
    for (var m = 0; m < numRows; m ++) {
        for (var n = 0; n < arr[m].length; n++) {
            result += arr[m][n];
        }                
    }
};
```

网上看到还有一种更高效的方法，然而没看懂。。。留待研究:

我们注意到每一行的数字的索引是有规律的！

第一行和最后一行中索引相差是8, 即（5-1)＊2

第二行时： 1+6 = 7,  7 + 2 = 9;  9 + 6 = 15;  15 + 2 = 17. ...   即在 6和2交替进行，逐个确定下一个索引的位置。  注意到 6+2 = 8， 2 ＝ 2*1(索引值)

第三行时：2+4 = 6;  6 + 4 = 10; 10+4 = 14 ...   都是4, 可以理解为 是 4 和4 交替。 注意到 4+4 = 8；   4 ＝ 2*2(索引值)

第四行时： 3+2 = 5; 5+6＝10... 在6 和2之间交替。注意到 6+2 = 8；   4 ＝ 2*3(索引值)

这规律可进行推广到第一行和最后一行。

所以根据当前位置，可以得到下一个字符的位置信息。可直接获取。 

这算法的时间复杂度为O(n), 空间复杂度为 O(1)