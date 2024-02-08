---
comments: true
---

# 704.二分查找 & 27.移除元素  

!!! success "Luck"

    荣幸在B站24.01-24[抽奖](https://www.bilibili.com/opus/889403979472044032?spm_id_from=333.999.0.0#:~:text=%E6%81%AD%E5%96%9C%E8%BF%99%E4%B8%A4%E4%BD%8D%E5%BD%95%E5%8F%8B%E4%B8%AD%E5%A5%96%EF%BC%8CB%E7%AB%99%E7%A7%81%E4%BF%A1%E6%88%91%E6%8B%89%E4%BD%A0%E5%85%A5%E7%BE%A4%E5%93%88%20%40H%2DYiheng%20%40%E8%9C%82%E8%9C%9C%E5%8A%A0%E7%89%9B%E5%A5%B6%E5%90%97)中了Carl哥的算法训练营！(😆)  -->  [参考网站](https://programmercarl.com/)

    陆陆续续学过一些算法，争取坚持系统学一遍！



## [704.二分查找](https://leetcode.cn/problems/binary-search/)

第一想法：暴力求呗，因为是升序的，直接循环找即可 
```c++ linenums="1"
class Solution {
public:
    int search(vector<int>& nums, int target) {
        for (int i = 0; i < nums.size(); i ++) {
            if (nums[i] == target) {
                return i;
            }
        }
        return -1;
    } 
};
```
但题目是 **二分查找** 呀！而且数组有序，定考虑二分 (区间要注意!)
```c++ linenums="1" hl_lines="3"
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1; // index 
        
        while(l <= r) {
            int mid = l + ((r - l) >> 1); // 防溢出

            if (nums[mid] > target) r = mid - 1;
            else if (nums[mid] < target) l = mid + 1;
            else return mid;
        }
        return -1;
    }
};
```

## [27.移除元素](https://leetcode.cn/problems/remove-element/)

原地移除等于`val`的元素 & 空间要求$O(1)$，最终返回新的数组长度

原地如何移除？数组元素能删除？ --> NO，本质即 **覆盖** 呗!

1. 想法: 暴力 --> 循环找到目标val，循环后一个先前进行覆盖
```C++ linenums="1"
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i < size; i ++) {
            if (nums[i] == val) {
                for (int j = i + 1; j < size; j ++) 
                    nums[j - 1] = nums[j];
                i --;
                size --;
            }
        }
        return size; 
    }
};
```

2. ==快慢指针==  --> [ref](https://programmercarl.com/0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95,-%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95), [动图参考](https://code-thinking.cdn.bcebos.com/gifs/27.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0-%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95.gif)
    - 对暴力进行优化，利用双指针降低复杂度（即一层循环$O(n)$

^^快指针来索引新数组需要的元素，更新到慢指针索引上，最终返回慢指针即可^^（太强了!!!
```c++ linenums="1"
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast ++) {
            if (nums[fast] != val) nums[slow ++] = nums[fast];
        }
        return slow;
    }
};
```