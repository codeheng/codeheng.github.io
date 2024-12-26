---
comments: true
---

# 二分查找 & 移除元素 & 有序数组的平方

??? success "Luck"

    荣幸在B站24.01-24[抽奖](https://www.bilibili.com/opus/889403979472044032?spm_id_from=333.999.0.0#:~:text=%E6%81%AD%E5%96%9C%E8%BF%99%E4%B8%A4%E4%BD%8D%E5%BD%95%E5%8F%8B%E4%B8%AD%E5%A5%96%EF%BC%8CB%E7%AB%99%E7%A7%81%E4%BF%A1%E6%88%91%E6%8B%89%E4%BD%A0%E5%85%A5%E7%BE%A4%E5%93%88%20%40H%2DYiheng%20%40%E8%9C%82%E8%9C%9C%E5%8A%A0%E7%89%9B%E5%A5%B6%E5%90%97)中了Carl哥的算法训练营！(😆)  -->  [参考网站](https://programmercarl.com/)

    陆陆续续学过一些算法，争取坚持系统学一遍！

    24.12.25-[B站抽奖](https://t.bilibili.com/1014739496055341057?spm_id_from=333.999.0.0)那么幸运又中了

    ~~上次打卡到DP太难，遂放弃~~，作为算法菜鸟希望这次尽可能坚持！

    **PS: 准备这次用Java，虽然主要是算法思想并没太大的区别**
    
    - 但因为也快秋招了，为了最后起码能保底拿个offer，思来想去还是Java可能更合适一些
    
    （即便很想研究C++，但感觉时间很不够了，论文还没弄出来而且C++基础并没很好，最重要的C++就业门槛更高一些，而Java开发选择面更广一些，甚至银行等国企可能还有机会）


## [704.二分查找](https://leetcode.cn/problems/binary-search/)

第一想法：暴力求呗，因为是升序的，直接循环找即可 

=== "Java"

    ```java linenums="1"
    class Solution {
        public int search(int[] nums, int target) {
            for(int i = 0; i < nums.length; i ++)
                if(nums[i] == target) return i;
            return -1;
        }
    }
    ```

=== "C++"

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

=== "Java"

    ```java linenums="1"
    class Solution {
        public int search(int[] nums, int target) {
            int l = 0, r = nums.length - 1;
            while (l <= r) {
                int mid = (l + r) >> 1;
                if (nums[mid] > target) r = mid - 1;
                else if(nums[mid] < target) l = mid + 1;
                else return mid;
            }
            return -1;
        }
    }
    ```

=== "C++"

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

=== "Java"

    ```java linenums="1"
    class Solution {
        public int removeElement(int[] nums, int val) {
            int n = nums.length;
            for (int i = 0; i < n; i ++) {
                if(nums[i] == val) {
                    for(int j = i + 1; j < n; j ++) nums[j - 1] = nums[j];
                    i --;
                    n --;
                }
            }
            return n;
        }
    }
    ```

=== "C++"

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

=== "Java" 

    ```java linenums="1"
    class Solution {
        public int removeElement(int[] nums, int val) {
            int s = 0;
            for (int f = 0; f < nums.length; f ++)
                if (nums[f] != val) nums[s ++] = nums[f];
            return s;
        }
    }
    ```

=== "C++"

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

## [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

第一想法: 对每个数进行平方，然后排序即可

=== "Java"

    ```java linenums="1"
    class Solution {
        public int[] sortedSquares(int[] nums) {
            for(int i = 0; i < nums.length; i ++)  
                nums[i] = nums[i] * nums[i];
            Arrays.sort(nums); // 对array排序
            return nums;
        }
    }
    ```

=== "C++"

    ```c++ linenums="1" hl_lines="5"
    vector<int> sortedSquares(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i ++) {
            nums[i] = nums[i] * nums[i];
        }
        sort(nums.begin(), nums.end());
        return nums;
    }
    ```

时间复杂度: $O(n + nlogn)$

2.双指针 --> 因为有序，最大值必在两端，两边扫描比较，开个数组，空间换时间([动图](https://code-thinking.cdn.bcebos.com/gifs/977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.gif))

=== "Java" 

    ```java linenums="1"
    class Solution {
        public int[] sortedSquares(int[] nums) {
            int[] res = new int[nums.length];
            int i = 0, j = nums.length - 1, k = nums.length - 1;
            while(i <= j) {
                if(nums[i] * nums[i] < nums[j] * nums[j]) {
                    res[k --] = nums[j] * nums[j];
                    j --;
                }
                else {
                    res[k --] = nums[i] * nums[i];
                    i ++;
                }
            }
            return res;
        }
    }
    ```

=== "C++"

    ```c++ linenums="1"
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> vec = nums;
        int i = 0, j = nums.size() - 1, k = nums.size() - 1;
        while (i <= j) {
            if (nums[i] * nums[i] < nums[j] * nums[j]) {
                vec[k --] = nums[j] * nums[j];
                j --;
            } else {
                vec[k --] = nums[i] * nums[i];
                i ++;
            }
        }
        return vec;
    }
    ```