## [剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)

### 题目描述

> 请实现一个函数，把字符串 s 中的每个空格替换成"%20"。
>
> 示例 1：
>
> 输入：s = "We are happy."
> 输出："We%20are%20happy."
>
>
> 限制：
>
> 0 <= s 的长度 <= 10000
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode.cn/problems/ti-huan-kong-ge-lcof

### 解题思路

遍历字符串，遇到**空格**转换成`%20`写入新字符串,  非空格时顺序写入即可

### 代码如下

```go
//一趟遍历
//时间复杂度O(n) 空间复杂度O(n+2m){开辟新空间存储结果字符串}
func replaceSpace(s string) string {
    //特判
    if len(s) == 0 {
        return s
    }
    result_str := []byte{}
    for i:=0; i<len(s); i++ {
        if s[i] != ' ' {
            result_str = append(result_str, s[i])
        } else {
            result_str = append(result_str, []byte("%20")...)
        }
    }
    return string(result_str)
}
```

