---
title: LeetCode 探索初级算法
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

LeetCode 探索初级算法

---

<!-- more -->

## 1 字符串

### 1.1 [反转字符串](https://leetcode-cn.com/problems/reverse-string/submissions/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组char[]的形式给出。不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用O(1)的额外空间解决这一问题。你可以假设数组中的所有字符都是ASCII码表中的可打印字符。

**示例**

```text
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**解法：双指针法**

设置双指针i、j分别指向头尾，交换头尾后各向中间进1.

```python
def reverseString(s):
    i, j = 0, len(s) - 1
    while i < j:
        s[i], s[j] = s[j], s[i]
        i += 1
        j -= 1
    return s
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

此题还可利用内置的`reverse()`函数或者`s[::-1]`来得到反转后的字符串，但这过于投机取巧。

### 1.2 [整数反转](https://leetcode-cn.com/problems/reverse-integer/)

给你一个32位的有符号整数$x$，返回将$x$中的数字部分反转后的结果。
如果反转后整数超过 32 位的有符号整数的范围$[−2^{31}, 2^{31}−1]$，就返回$0$。

**示例**

```text
输入: -123
输出: -321
```

**解法：栈**

每次对$x$除$10$取余，并将余数乘10累加到res中。

```python
def reverse(x):
    # sign为正负号
    sign = -1 if x < 0 else 1
    # 取整数部分，相当于取绝对值
    x = x * sign
    res = 0
    while x != 0:
        res = res * 10 + x % 10
        x = x // 10
    res = res * sign
    return  res if -2**31 < res < 2**31 - 1 else 0
```

**复杂度分析**

- 时间复杂度：$O(log(x))$
- 空间复杂度：$O(1)$

此题还可将数字转换为字符串后直接逆转字符串，但这显然不是本题想要考察的内容。


### 1.3 [字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回-1

**示例**

```text
输入：s = "leetcode"
输出：0.

输入：s = "loveleetcode",
输出：2.
```

**解法：hash表存储数频**

两边遍历字符串：第一遍使用hash映射出每个字符出现的频次，第二遍找出频次为1的字符并返回索引。

```python
def firstUniqChar(s):
    frequency = collections.Counter(s)
    for i, c in enumerate(s):
        if frequency[c] == 1:
            return i
    return -1
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(\Sigma)$，$\Sigma=26$为字符表大小


### 1.4 [有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/submissions/)

给定两个字符串s和t，编写一个函数来判断t是否是s的字母异位词，即s和t中每个字符出现的次数都相同。

**示例**

```text
输入: s = "anagram", t = "nagaram"
输出: true

输入: s = "rat", t = "car"
输出: false

输入: s = "aacc", t = "ccac"
输出: false
```

**解法：hash表**

可以构建两个哈希表，分别保存s和t的词频，并比较这两个哈希表是否相同

```python
def isAnagram(s, t):
    return collections.Counter(s) == collections.Counter(t)
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(2S)$，其中$S$为字符串长度

此外，本题还可对两个字符串进行排序，并比较排序后的字符串是否相同，即`return sorted(s) == sorted(t)`，其时间复杂度为$O(n\log n)$，空间复杂度为$O(\log n)$

### 1.5 [验证回文字符串](https://leetcode-cn.com/problems/valid-palindrome/)

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写

**示例**

```text
输入: "A man, a plan, a canal: Panama"
输出: true

输入: "race a car"
输出: false
```

**解法：双指针法**

利用双指针分别从头和尾进行比较，两个关键函数：isalnum()判断是否为字母数字，以及lower()转换成小写字母

```python
def isPalindrome(s):
    i, j = 0, len(s) - 1
    while i < j:
        # 判定条件i<j的作用是防止在s=", . ?~"这样的字符串中因i>j而越界
        while i < j and not s[i].isalnum():
            i += 1
        while i < j and not s[j].isalnum():
            j -= 1
        if s[i].lower() != s[j].lower():
            return False
        i += 1
        j -= 1
    return True
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$


## 2 数组

### 2.1 [旋转数组](https://leetcode-cn.com/problems/rotate-array/)

给定一个数组，将数组中的元素向右移动k个位置，其中k是非负数

**示例**

```plain
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]

输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
```


**解法1：暴力法**

旋转k次，每次将数组向右旋转移动一位

```C++
void rotate(vector<int>& nums, int k) {
    int pre, tmp;
    for (int i = 0; i < k; ++i) {
        int pre = nums[nums.size() - 1];
        for (int j = 0; j < nums.size(); ++j) {
            tmp = nums[j];
            nums[j] = pre;
            pre = tmp;
        }
    }
}
```

**复杂度分析**

- 时间复杂度：每次都将移动n个元素，总共移动k次，时间复杂度为$O(n*k)$
- 空间复杂度：仅使用了两个辅助变量，空间复杂度为$O(1)$

**解法2：使用额外数组**

使用一个额外的数组，将原数组第i个位置上的元素放于新数组第(i+k)%array.size()位置上.

```C++
void rotate(vector<int>& nums, int k) {
    vector<int> arr = nums;
    for (int i = 0; i < nums.size(); ++i)
        arr[(i + k) % nums.size()] = nums[i];
    for (int i = 0; i < nums.size(); ++i)
        nums[i] = arr[i];
}
```

**复杂度分析**

- 时间复杂度：由于遍历了两次数组，因此时间复杂度为$O(2*n)$
- 空间复杂度：由于使用了临时数组，因此空间复杂度$O(n)$

**解法3：使用环状替换**

如果我们直接把每一个数字放到它最后的位置，但这样的后果是遗失原来的元素。因此，我们需要把被替换的数字保存在变量$temp$里面。然后，我们将被替换数字$(temp)$放到它正确的位置，并继续这个过程n次，n是数组的长度。这是因为我们需要将数组里所有的元素都移动。但是，这种方法可能会有个问题，如果$n%k==0$，其中$k=k%n$(因为如果k大于n，移动k次实际上相当于移动$k%n$次)。这种情况下，我们会发现在没有遍历所有数字的情况下回到出发数字。此时，我们应该从下一个数字开始再重复相同的过程。

现在，我们看看上面方法的证明。假设，数组里我们有n个元素并且k是要求移动的次数。更进一步，假设$n%k=0$。第一轮中，所有移动数字的下标i满足$i%k==0$。这是因为我们每跳k步，我们只会到达相距为k个位置下标的数。每一轮，我们都会移动$n/k$个元素。下一轮中，我们会移动满足$i%k==1$的位置的数。这样的轮次会一直持续到我们再次遇到$i%k==0$的地方为止，此时i=k。此时在正确位置上的数字共有$k*(n/k)=n$个。因此所有数字都在正确位置上

```C++
void rotate(vector<int>& nums, int k) {
    k = k % nums.size();
    int count = 0;
    for (int start = 0; count < nums.size(); start++) {
        int current = start;
        int prev = nums[start];
        do {
            int next = (current + k) % nums.size();
            int temp = nums[next];
            nums[next] = prev;
            prev = temp;
            current = next;
            count++;
        } while (start != current);
    }
}
```

**复杂度分析**

- 时间复杂度：只遍历了一遍数组，$O(n)$
- 空间复杂度：只用了常数个额外空间，$O(1)$

**解法4：数组反转**

先将所有数组反转，再反转前k个数组元素，最后再反转后n-k个数组元素

```C++
void rotate(vector<int>& nums, int k) {
    if (k > nums.size())
        k = k % nums.size();
    reverse(nums.begin(), nums.end());
    reverse(nums.begin(), nums.begin() + k);
    reverse(nums.begin() + k, nums.end());
}
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$，没有使用额外空间

### 2.2 [从数组中删除重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

给定一个排序数组，你需要在**原地删除重复出现的元素**，使得每个元素只出现一次，返回移除后数组的新长度。不要使用额外的数组空间，你必须在原地修改输入数组并在使用O(1)额外空间的条件下完成.

**解法：双指针法**

数组完成排序后，可以放置两个指针i和j，其中i为慢指针，j为快指针，当nums[i]==nums[j]时，表示nums[i]到nums[j]之间为重复项，此时增加j以跳过重复项。当nums[i]!=nums[j]时，将num[j]赋值给nums[i+1]，并增加i。以数组[0, 1, 1, 2, 3, 3, 3]为例：

```text
[0,1,1,2,3,3,3] ➜ [0,1,1,2,3,3,3] ➜ [0,1,1,2,3,3,3] ➜
 i                   i                   i
 j                   j                 j

[0,1,1,2,3,3,3] ➜ [0,1,2,2,3,3,3] ➜ [0,1,2,3,3,3,3] ➜ ...
       i                 i                   i
   j                   j                   j
```

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i = 0;
        for (int j:nums)
            if (!i || j > nums[i - 1])
                nums[i++] = j;
        return i;
    }
};
```

**复杂度分析**

- 时间复杂度：仅遍历一次数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.3 [买卖股票的最佳时机II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

给定一个数组，它的第i个元素是一支给定股票第i天的价格。设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例**

```text
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第2天（股票价格=1）的时候买入，在第3天（股票价格=5）的时候卖出,
    　这笔交易所能获得利润=5-1=4。随后，在第4天（股票价格=3）的时候买入，
    　在第5天（股票价格=6）的时候卖出,这笔交易所能获得利润=6-3=3。

输入: [1,2,3,4,5]
输出: 4
解释: 在第1天（股票价格=1）的时候买入，在第5天（股票价格=5）的时候卖出,
    　这笔交易所能获得利润=5-1=4。注意你不能在第1天和第2天接连购买股票，
    　之后再将它们卖出。因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**解法：暴力法**

首先，当数组长度小于2时，不能进行一次完整的买卖，返回0，当数组长度大于2时，分两种情况讨论：

- 一开始股市是升的：这种情况下从一开始就要购入，并将购买tag置为true。随后继续遍历数组，当遇到最高点时抛售股票。反复执行购买抛售，直到最后一天。如果最后一天tag仍然为true(对应示例2)，此时需要抛售，因此可在循环外加一个if判断；
- 一开始股市是降的：这种情况下就一直遍历数组，直到最低谷，随后将其视为第一种情况；

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() < 2)
            return 0;
        int profit = 0;
        bool has_buy = false;

        if (prices[0] < prices[1]) {
            profit -= prices[0];
            has_buy = true;
        }

        for (int i = 1; i < prices.size() - 1; ++i) {
            if (has_buy == true && prices[i] > prices[i + 1]) {
                profit += prices[i];
                has_buy = false;
            } else if (has_buy == false && prices[i] < prices[i + 1]) {
                profit -= prices[i];
                has_buy = true;
            }
        }

        if (has_buy == true)
            profit += *(prices.end() - 1);
        return profit;
    }
};
```

**复杂度分析**

- 时间复杂度：仅遍历一次数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.4 [存在重复](https://leetcode-cn.com/problems/contains-duplicate/)

给定一个整数数组，判断是否存在重复元素。如果任何值在数组中出现至少两次，函数返回true。如果数组中每个元素都不相同，则返回false。

**解法：使用unorder_set**

使用unorder_set，unorder_set是基于hashtable的按键唯一存储关联容器，容器内元素无序。因此可以遍历数组，如果遍历到的数在unorder_set不存在，则将其插入unorder_set，否则就返回true。

```C++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> set;
        for (auto it : nums)
        {
            if (set.count(it) > 0)
                return true;
            else
                set.insert(it);
        }
        return false;
    }
};
```

**复杂度分析**

- 时间复杂度：仅遍历一次数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.5 [只出现一次的数字](https://leetcode-cn.com/problems/single-number/)

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**解法：使用异或**

该题可以使用暴力解法，但其时空消耗过大。此处介绍一种数学方法：由于除了欲找出的那个数字外，其余数字均出现两次，因此我们可以利用异或操作。异或的性质是，任何一个数字异或自己都为0，因此我们可以异或所有的数字而得到目标值。

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (auto i : nums)
            ans ^= i;
        return ans;
    }
};
```

**复杂度分析**

- 时间复杂度：仅遍历一次数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.6 [两个数组的交集II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

给定两个数组，编写一个函数来计算它们的交集。

**示例**

```text
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]

输入: nums1 = [1,2,3,4,5], nums2 = [2,3,5]
输出: [2,3,5]
```

**解法：使用find()函数**

使用std::find()函数，如果从nums2中找到了和nums1中相同的数字，则放进res中，并从nums2中删去该数字

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        for(int i=0;i<nums1.size();i++) {
            auto it=find(nums2.begin(),nums2.end(),nums1[i]);
            if(it!=nums2.end()) {
                res.push_back(*it);
                nums2.erase(it);
            }
        }
        return res;
    }
};
```

**复杂度分析**

- 时间复杂度：find函数遍历nums2整个数组，外层for循环遍历nums1数组，因此时间复杂度为$O(mn) \sim O(n^2)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.7 [加一](https://leetcode-cn.com/problems/plus-one/)

给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。你可以假设除了整数0之外，这个整数不会以零开头。

**示例**

```text
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字123。
```

**解法：反向遍历**

首先将数组最后一位加一，随后从后向前遍历直到第1个数，如果有一个数等于10，则将其变为0并将其前一位加一，退出循环后再判断第0个数是否为10，若为10则变为0并在数组首部插入1.

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        digits[digits.size() - 1]++;
        for (auto i = digits.size() - 1; i > 0; --i) {
            if (digits[i] == 10) {
                digits[i] = 0;
                digits[i - 1]++;
            }
        }
        if (digits[0] == 10) {
            digits[0] = 0;
            digits.insert(digits.begin(), 1);
        }
        return digits;
    }
};
```

**复杂度分析**

- 时间复杂度：for循环遍历一遍数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.8 [移动零](https://leetcode-cn.com/problems/move-zeroes/)

给定一个数组nums，编写一个函数将所有0移动到数组的末尾，同时保持非零元素的相对顺序。

**示例**

```text
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
说明: 必须在原数组上操作，不能拷贝额外的数组，尽量减少操作次数。
```

**解法：双指针法**

设置快指针i和慢指针j，其中i指针从头开始遍历，遇到0则继续遍历，遇到非0则停下，并交换nums[i]和nums[j]，慢指针j和i同步从头开始遍历，遇到0则停下并等待i，遇到非0则加1.

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i = 0, j = 0;
        while (i < nums.size()) {
            if (nums[i] == 0)
                i++;
            else {
                swap(nums[i], nums[j]);
                i++;
                j++;
            }
        }
    }
};
```

**复杂度分析**

- 时间复杂度：for循环遍历一遍数组，时间复杂度为$O(n)$
- 空间复杂度：仅使用了常数个辅助变量，空间复杂度为$O(1)$

### 2.9 [两数之和](https://leetcode-cn.com/problems/two-sum/)

给定一个整数数组nums和一个目标值target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

**示例**

```text
输入: nums = [2, 7, 11, 15], target = 9
输出: [0,1]
说明: 因为 nums[0] + nums[1] = 2 + 7 = 9,所以返回 [0, 1]
```

**解法1：暴力法**

暴力法很简单，遍历每个元素x，并查找是否存在一个值与target−x相等的目标元素

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ans;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[i] + nums[j] == target) {
                    ans.push_back(i);
                    ans.push_back(j);
                }
            }
        }
        return ans;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$

**解法2：两遍哈希表**

以空间换时间，使用hash表保存数组，在想hash表插入数的同时查找对应值是否存在

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hash_set;
        vector<int> ans;
        for (int i = 0; i < nums.size(); ++i) {
            if (hash_set.count(target - nums[i]) > 0) {
                ans.push_back(hash_set[target - nums[i]]);
                ans.push_back(i);
                break;
            }
            hash_set[nums[i]] = i;
        }
        return ans;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$

### 2.10 [有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

**示例**

```text
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**解法：一次遍历**

使用hash表。思想是遍历每一个格子，然后判断该格子的值是否已在hash表中，若已存在则返回false，否则记录入hash表中

```c++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        vector<unordered_map<int,int>> rows(9), cols(9), boxes(9);
        for(int i = 0; i < 9; ++ i) {
            for(int j = 0; j < 9; ++ j) {
                int box_index = (i / 3) * 3 + j / 3;
                char n = board[i][j];
                if(n != '.') {
                    if(rows[i].count(n) || cols[j].count(n) || boxes[box_index].count(n))
                        return false;
                    rows[i][n] = 1;
                    cols[j][n] = 1;
                    boxes[box_index][n] = 1;
                }
            }
        }
        return true;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(1)$
- 空间复杂度：$O(1)$

### 2.11 [旋转图像](https://leetcode-cn.com/problems/rotate-image/)

给定一个n×n的二维矩阵表示一个图像。将图像顺时针旋转90度。

**示例**

```text
给定matrix= 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**解法：翻转**

先转置数组，即swap(nums[i][j], nums[j][i])，再翻转每一行。该方法已经达到了最优时间复杂度$O(n^2)$

```c++
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        for (int i = 0; i < matrix.size(); ++i)
            for (int j = i; j < matrix[0].size(); ++j)
                swap(matrix[i][j], matrix[j][i]);
        for (int i = 0; i < matrix.size(); ++i)
            reverse(matrix[i].begin(), matrix[i].end());
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n^2)$
- 空间复杂度：$O(1)$