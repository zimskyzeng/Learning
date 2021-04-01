# 6-N字形变换-M

### 题目描述

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 N 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"PAHNAPLSIIGYIR"`。



### 解题思路





### 算法实现

```python
class Solution(object):
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        if numRows <=1:
            return s

        # 长度
        size = len(s)
        # 结果
        res = ""

        for i in range(numRows):
            tmp = 0
            while True:
                if i == 0 or i == numRows -1:
                    mid = (2 * numRows -2) * tmp + i
                    if mid < size:
                        res = res + s[mid]
                        tmp += 1
                    else:
                        break
                else:
                    mid1 = (2 * numRows -2) * tmp + i
                    if mid1 < size:
                        res = res + s[mid1]
                    else:
                        break
                    mid2 = (2 * numRows -2) * tmp + ((2 * numRows -2) - i)
                    if mid2 < size:
                        res = res + s[mid2]
                        tmp += 1
                    else:
                        break
        return res
```

