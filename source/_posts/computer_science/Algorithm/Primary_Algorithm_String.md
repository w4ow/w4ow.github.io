---
title: LeetCode 探索初级算法 字符串 
date: 2019-08-07
updated: 2019-08-07
mathjax: true
meta:
    date: false
categories: 
    - Computer Science
    - Algorithm
tags:
    - LeetCode
    - String
---

1. [反转字符串](https://leetcode-cn.com/problems/reverse-string/submissions/)
2. [整数反转](https://leetcode-cn.com/problems/reverse-integer/)
3. [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)
4. [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/submissions/)
5. [验证回文字符串](https://leetcode-cn.com/problems/valid-palindrome/)
6. [字符串转换整数(atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)
7. [实现strStr()](https://leetcode-cn.com/problems/implement-strstr/)
8. 报数
9. 最长公共前缀

---

<!-- more -->

## 1. 反转字符串

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组char[]的形式给出。不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用O(1)的额外空间解决这一问题。你可以假设数组中的所有字符都是ASCII码表中的可打印字符。

**示例**

```text
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

### 解法1：双指针法

设置双指针i、j分别指向头尾，交换头尾后各向中间进1.

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0, j = s.size() - 1;
        while (i < j) {
            swap(s[i], s[j]);
            i++;
            j--;
        }
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$


### 解法2：reverse函数

直接使用reverse()函数

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        reverse(s.begin(), s.end());
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

## 2. 整数反转

给出一个32位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例**

```text
输入: 123
输出: 321
```

### 解法：模拟栈

每次对x取%10余，并将余数×10累加到res中

```c++
class Solution {
public:
    int reverse(int x) {
        int res = 0, cur;
        while (x != 0) {
            cur = x % 10;
            x /= 10;
            if (res > INT_MAX/10 || (res == INT_MAX / 10 && cur > 7)) return 0;
            if (res < INT_MIN/10 || (res == INT_MIN / 10 && cur < -8)) return 0;
            res = res * 10 + cur;
        }
        return res;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(log(x))$
- 空间复杂度：$O(1)$

## 3. 字符串中的第一个唯一字符

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回-1

**示例**

```text
s = "leetcode"
返回0.

s = "loveleetcode",
返回2.
```

### 解法：hash表

使用map保存字符-位置键对，且这一键对的值为该字符出现的次数。随后从头按字符出现的次序遍历map，第一个键值为1的便是解。

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<char,int> hash;
        for (auto i : s) {
            hash[i]++;
        }
        for (int i = 0; i < s.size(); ++i)
            if (hash[s[i]] == 1)
                return i;
        return -1;
    }
};

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$


```

## 4. 有效的字母异位词

给定两个字符串s和t，编写一个函数来判断t是否是s的字母异位词。

**示例1**

```text
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例2**

```text
输入: s = "rat", t = "car"
输出: false
```

**示例3**

```text
输入: s = "aacc", t = "ccac"
输出: false
```

### 解法：hash表

使用26大小的数组作为hash表，同时遍历s和t，并在对应位分别++和--。最后再遍历一遍hash表，如果存在不为0的元素，则说明s和t字符不对应，即不是异位词

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size())
            return false;
        int count[26] = {0};
        for (int i = 0; i < s.size(); ++i) {
            count[s[i] - 'a']++;
            count[t[i] - 'a']--;
        }
        for (auto i : count)
            if (i != 0)
                return false;
        return true;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

## 5. 验证回文字符串

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写

**示例1**

```text
输入: "A man, a plan, a canal: Panama"
输出: true
```

**示例2**

```text
输入: "race a car"
输出: false
```

### 解法：双指针法

利用双指针分别从头和尾进行比较，两个关键函数：isalnum()判断是否为字母数字，以及tolower()转换成小写字母

```c++
class Solution {
public:
    bool isPalindrome(string s) {
        int i = 0, j = s.size() - 1;
        while (i < j) {
            while (i < j && !isalnum(s[i]))
                i++;
            while (i < j && !isalnum(s[j]))
                j--;
            if (tolower(s[i++]) != tolower(s[j--]))
                return false;
        }
        return true;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

## 6. 字符串转换整数(atoi)

请你来实现一个atoi函数，使其能将字符串转换成整数。

### 解法

这题实在是太麻烦了，各种意想不到的输入都需要考虑到，因此这里提供一个懒人方法，使用sstream库。

```c++
class Solution {
public:
    int myAtoi(string str) {
        int digit = 0;
        istringstream is(str);
        is >> digit;
        return digit;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

## 7. 实现strStr()

给定一个haystack字符串和一个needle字符串，在haystack字符串中找出needle字符串出现的第一个位置(从0开始)。如果不存在，则返回-1

**示例**

```text
输入: haystack = "hello", needle = "ll"
输出: 2
```

### 解法1：暴力求解

从头至尾遍历一遍

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if needle == "":
            return 0
        for i in range(len(haystack)) :
            if needle == haystack[i : i + len(needle)]:
                return i
        return -1
```

**复杂度分析**

- 时间复杂度：$O(nm)$
- 空间复杂度：$O(1)$

## 解法2：KMP算法

```c++
class Solution {
public:
    vector<int> getnext(string str) {
        int len=str.size();
        vector<int> next;
        next.push_back(-1);//next数组初值为-1
        int j=0,k=-1;
        while(j<len-1) {
            if(k==-1||str[j]==str[k]) {
                j++;
                k++;
                if(str[j]!=str[k])
                    next.push_back(k);
                else
                    next.push_back(next[k]);
            } else {
                k=next[k];
            }
        }
        return next;
    }

    int strStr(string haystack, string needle) {
        if(needle.empty())
            return 0;

        int i=0;//源串
        int j=0;//子串
        int len1=haystack.size();
        int len2=needle.size();
        vector<int> next;
        next=getnext(needle);
        while((i<len1)&&(j<len2)) {
            if((j==-1)||(haystack[i]==needle[j])) {
                i++;
                j++;
            } else {
                j=next[j];//获取下一次匹配的位置
            }
        }
        if(j==len2)
            return i-j;
        return -1;
    }
};
```

- 时间复杂度：$O(n+m)$
- 空间复杂度：$O(n)$

## 解法3：BM算法

```c++
class Solution {
public:
    void get_bmB(string& T,vector<int>& bmB) {
        int tlen=T.size();
        for(int i=0;i<256;i++) {
            bmB.push_back(tlen);
        }
        for(int i=0;i<tlen-1;i++) {
            bmB[T[i]]=tlen-i-1;
        }
    }

    void get_suff(string& T,vector<int>& suff) {
        int tlen=T.size();
        int k;
        for(int i=tlen-2;i>=0;i--) {
            k=i;
            while(k>=0&&T[k]==T[tlen-1-i+k])
                k--;
            suff[i]=i-k;
        }
    }

    void get_bmG(string& T,vector<int>& bmG) {
        int i,j;
        int tlen=T.size();
        vector<int> suff(tlen+1,0);
        get_suff(T,suff);//suff存储子串的最长匹配长度
        //初始化 当没有好后缀也没有公共前缀时
        for(i=0;i<tlen;i++)
            bmG[i]=tlen;
        //没有好后缀 有公共前缀 调用suff 但是要右移一位 类似KMP里的next数组
        for(i=tlen-1;i>=0;i--)
            if(suff[i]==i+1)
                for(j=0;j<tlen-1;j++)
                    if(bmG[j]==tlen)//保证每个位置不会重复修改
                        bmG[j]=tlen-1-i;
        //有好后缀 有公共前缀
        for(i=0;i<tlen-1;i++)
            bmG[tlen-1-suff[i]]=tlen-1-i;//移动距离
    }

    int strStr(string haystack, string needle) {

        int i=0;
        int j=0;
        int tlen=needle.size();
        int slen=haystack.size();

        vector<int> bmG(tlen,0);
        vector<int> bmB;
        get_bmB(needle,bmB);
        get_bmG(needle,bmG);

        while(i<=slen-tlen) {
            for(j=tlen-1;j>-1&&haystack[i+j]==needle[j];j--);
            if(j==(-1))
                return i;
            i+=max(bmG[j],bmB[haystack[i+j]]-(tlen-1-j));
        }
        return -1;
    }
};
```

- 时间复杂度：最差$O(n+m)$，最好$O(n)$
- 空间复杂度：$O(n)$