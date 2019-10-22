## 344. 反转字符串
[题目地址](https://leetcode-cn.com/problems/reverse-string/)

**编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。**

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

示例 1：

> 输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]

示例 2：

>输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]

思路：经典双指针解法，设置两个指针一个指向头一个指向尾，向中间移动两个指针依次交换元素，直到指针相遇。
```go
func reverseString(s []byte)  {
    left,right:=0,len(s)-1//头尾双指针
    ////头指针向后移动尾指针向前移动，当没有遇到的时候双方交换元素
    for left<right&&len(s)!=0{
        s[left],s[right]=s[right],s[left]
        left++
        right--
    }
}
```
![](https://img-blog.csdnimg.cn/20190810174239279.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)
## 7. 整数反转
[题目地址](https://leetcode-cn.com/problems/reverse-integer/)

**给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。**

示例 1:

> 输入: 123
输出: 321
 
 示例 2:
> 输入: -123
输出: -321

示例 3:

>输入: 120
输出: 21

注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

**思路：从右往左依次取数，组合成新的数字。**

```go
func reverse(x int) int {
    result:=0
    for x!=0{
        result=result*10+(x%10)//对原数对10取余则可以得到最后一位数
        x/=10//左移
    }
    if result>math.MaxInt32||result<math.MinInt32{
        result = 0
    }
    return result
}
```
![](https://img-blog.csdnimg.cn/20190810175016254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)
## 387. 字符串中的第一个唯一字符
[题目地址](https://leetcode-cn.com/problems/first-unique-character-in-a-string)


**给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。**

案例:

>s = "leetcode"
返回 0.

>s = "loveleetcode",
返回 2.
 
注意事项：您可以假定该字符串只包含小写字母。

**思路：使用map统计单词中字母出现的字频，遍历字符串输出第一个字频为1的字符位置即可。**
```go
func firstUniqChar(s string) int {
    stmp:=[]byte(s)//字符串转字节数组
    sMap:=make(map[byte]int)
    for _,v:=range stmp{//利用map统计字频
        sMap[v]++
    }
    for i,v:=range stmp{//找出第一个字频为1的元素
        if sMap[v]==1{
            return i
        }
    }
    return -1
}
```
![](https://img-blog.csdnimg.cn/20190810175220881.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)
## 242. 有效的字母异位词
[题目地址](https://leetcode-cn.com/problems/valid-anagram)

**给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。**

示例 1:

> 输入: s = "anagram", t = "nagaram"
输出: true

示例 2:

> 输入: s = "rat", t = "car"
输出: false

说明:
你可以假设字符串只包含小写字母。

进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

**思路：比较笨的方法，使用两个Map统计字符频率，然后比较两个Map是否相同。**

```go
import "reflect"
func isAnagram(s string, t string) bool {
    sB:=[]byte(s)
    sT:=[]byte(t)
    sMapB:=make(map[byte]int)
    sMapT:=make(map[byte]int)
    for _,v:=range sB{
        sMapB[v]++
    }
    for _,v:=range sT{
        sMapT[v]++
    }
    if reflect.DeepEqual(sMapB,sMapT){
        return true
    }
    return false
}
```
![](https://img-blog.csdnimg.cn/20190810182725374.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)
评论区有种C巧妙方法备忘：
```c
bool isAnagram(char *s, char *t)
{
	int i, x[26] = { 0 }, y[26] = { 0 };
	for (i = 0; s[i] != '\0'; i++)	x[s[i] - 'a']++;	//建立 s 的字符表 x
	for (i = 0; t[i] != '\0'; i++)	y[t[i] - 'a']++;	//建立 t 的字符表 y
	for (i = 0; i < 26; i++)							//比较两字符表是否相同
		if (x[i] != y[i])	return false;
	return true;										//种类、个数均相同
}
```

## 125. 验证回文串
[题目地址](https://leetcode-cn.com/problems/valid-palindrome)

**给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。**

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:

> 输入: "A man, a plan, a canal: Panama"
输出: true

示例 2:
> 输入: "race a car"
输出: false

**思路：** 将给定的字符串转为字符数组，过滤掉非数字非小写字母元素，并前移所有有效字符。
如 `A man, a plan, a canal: Panama`过滤后为`amanaplanacanalpanamal: panama`，只考虑前面有效的位数。然后用双指针判断回文。
```go
func isPalindrome(s string) bool {
    sB:=[]byte(s)
    m:=0
    for i:=0;i<len(sB);i++{
        sB[i]|=32//位运算将大写转换为小写
        //过滤符合条件的小写字母和数字，将其前移
        if sB[i]>=97&&sB[i]<=122||sB[i]>=48&&sB[i]<=57{
            sB[m]=sB[i]
            m++//m为操作后有效长度尾部
        }
    }
    l,r:=0,m-1//双指针回文比较
    for l<r{
        if sB[l]!=sB[r]{
            return false
        }
        l++
        r--
    }
    return true
}
```
![](https://img-blog.csdnimg.cn/20190810183727914.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)

## 8. 字符串转换整数 (atoi)
[题目地址](https://leetcode-cn.com/problems/string-to-integer-atoi)

**请你来实现一个 atoi 函数，使其能将字符串转换成整数。**

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

> 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

示例 1:

> 输入: "42"
输出: 42

示例 2:

>输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。

示例 3:

> 输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。

示例 4:

>输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。

示例 5:

>输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。

**思路**：
这题需要考虑的情况比较多，需要把题目所有的情况都考虑进去。
- 因为符号位和有效数字位总会在一起的，故设置两个指针来标志有效区间
- 过滤串前空格（如果有的话）
- 如果前方有正负号，有效位的开始向后一位，并记录符号
- 后有效位是从前有效位开始遍历遇到的第一个非数字元素的位置
- 用Map做一个字符与数字的映射，遍历前面有效位的切片，构造新的数字
- 构造的过程中，如果数字出现溢出，则用有效位前的符号来决定取最大值还是最小值

```go
func myAtoi(str string) int {
    xflag:=1//记录符号
    sB:=[]byte(str)//转字节数组好处理
    l:=0//前有效位：left
    
    for i:=0;i<len(sB)&&sB[i]==32;i++{//跳过空格字符
        l++
    }
    
    if l==len(sB){//字符串全是空格的请款
        return 0
    }
    
    
    if sB[l]==43{//判断首位符号
        l++
    }else if sB[l]==45{
        xflag=-1
        l++
    }
    
    r:=l
    //找右界
    for i:=l;i<len(sB)&&sB[r]>=48&&sB[r]<=57;i++{
        r++
    }
    
    num:=make(map[byte]int)//字符与数字的映射
    var k byte=48
    for i:=0;i<10;i++{
        num[k]=i
        k++
    }
    
    result:=0
    for _,v:=range sB[l:r]{
        result=result*10+num[v]//构建新的数
        //溢出情况处理
        if result>math.MaxInt32&&xflag==1{
            return math.MaxInt32
        }else if result>math.MaxInt32&&xflag==-1{
            return math.MinInt32
        }
    }
    result*=xflag//符号位决定正负
    return result
}
```

## 28. 实现strStr()
[题目地址](https://leetcode-cn.com/problems/implement-strstr)

**实现 strStr() 函数。**

给定一个 `haystack` 字符串和一个 `needle` 字符串，在 `haystack` 字符串中找出 `needle` 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  `-1`。

示例 1:

```
输入: haystack = "hello", needle = "ll"
输出: 2
```
示例 2:
```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```
说明:

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 `0`。这与C语言的 `strstr()` 以及 Java的 `indexOf()` 定义相符。

**题解：**
```go
func strStr(haystack string, needle string) int {
    if len(needle)==0{
        return 0
    }
    if len(needle)>len(haystack){
        return -1
    }
    for i:=0;i<len(haystack);i++{
        if haystack[i]==needle[0]{
            result:=true
            for m:=0;m<len(needle);m++{
                if i+m==len(haystack){
                    return -1
                }
                result=result&&(haystack[i+m]==needle[m])
                if !result{
                    break
                }
            }
            if result{
                return i
            }
        }
    }
    return -1
}
```

## 38. 报数
[题目地址](https://leetcode-cn.com/problems/count-and-say/)

报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

`1.     1`
`2.     11`
`3.     21`
`4.     1211`
`5.     111221`
`1` 被读作  `"one 1"`  ("一个一") , 即 `11`。
`11 `被读作 `"two 1s" `("两个一"）, 即 `21`。
`21` 被读作 `"one 2",  "one 1"` （"一个二" ,  "一个一") , 即 `1211`。

给定一个正整数 `n（1 ≤ n ≤ 30）`，输出报数序列的第 n 项。

注意：整数顺序将表示为一个字符串。

示例 1:

>输入: 1
输出: "1"

示例 2:

>输入: 4
输出: "1211"


```go
func countAndSay(n int) string {
    s:="1"//第一个序列
    for i:=1;i<n;i++{//组装次数
        tmps:=""//暂存每次组装结果
        tmpc:=s[0]
        cnt:=0
        for j:=0;j<len(s);j++{//遍历上一个结果
            if tmpc!=s[j]{//遇到下一个不同的字符
                tmps+= strconv.Itoa(cnt) + string(tmpc)//组装上一个结果
                tmpc=s[j]
                cnt=1//重置频数
            }else{
                cnt++//字符频数
            }
        }
        tmps+= strconv.Itoa(cnt) + string(tmpc)//组装最后一个字符结果
        s=tmps//结果带给下一次
    }
    return s
}
```
![](https://img-blog.csdnimg.cn/20190811213625284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)

## 14. 最长公共前缀
[题目地址](https://leetcode-cn.com/problems/longest-common-prefix)

**编写一个函数来查找字符串数组中的最长公共前缀。**

如果不存在公共前缀，返回空字符串 ""。

示例 1:
```
输入: ["flower","flow","flight"]
输出: "fl"
```
示例 2:
```
输入: ["dog","racecar","car"]
输出: ""
```
解释: 输入不存在公共前缀。

说明:
所有输入只包含小写字母 `a-z` 。


**思路：**因为我们在比较过程中无法方便的拿到每个单词的长度，所以我们首先选取数组中长度最小的单词作为我们的标准单词（公共前缀长度一定不会超过最小单词）。然后用标准单词的每一位和其他单词的对应位去比较，每次循环比较都相等的字符说明是公共前缀，加入结果集字符串`result`，直到有一次不相等则返回结果串。另外注意处理空串和无相同前缀串。

```go
func longestCommonPrefix(strs []string) string {
    result:=""
    if len(strs)==0{//输入长度为空
        return result   
    }
    preWord:=strs[0]//第一个单词
    for _,v:=range strs{
        if len(v)<len(preWord){
            preWord=v
        }
    }
    for i:=0;i<len(preWord);i++{//遍历第一个单词的每一项
        for j:=0;j<len(strs);j++{//与其他单词的对应位进行比较
            if preWord[i]!=strs[j][i]{//若对应位不同
                return result 
            }
        }
        result+=string(preWord[i])
        if len(result)==len(preWord){
            break
        }
    } 
    return result
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190811213758969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MTY5Nzgx,size_16,color_FFFFFF,t_70)
应该存在更简单的解？求教。