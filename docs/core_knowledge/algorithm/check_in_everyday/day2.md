---
comments: true
---

# 长度最小子数组 & 螺旋矩阵 & 区间和

## [209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

> 描述: 正整数数组里面找 >= target的长度最小的连续子数组

1、暴力，两次循环找所有情况，比较更新子数组长度和结果

=== "Java"

    18/21 cases passed (N/A) **TLE**

    ```java linenums="1"
    class Solution {
        public int minSubArrayLen(int target, int[] nums) {
        int t = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i ++) {
                int sum = nums[i];
                if(sum >= target) return 1;
                for(int j = i + 1; j < nums.length; j ++) {
                    sum += nums[j];
                    if(sum >= target) {
                        t = Math.min(t, j - i + 1);
                        break;
                    }
                }
        }
        return t == Integer.MAX_VALUE ? 0 : t;
        }
    }
    ```


=== "C++"

    ```c++ linenums="1"
    int minSubArrayLen(int target, vector<int>& nums) {
        int max = 0x3f3f3f3f, res = max;
        for (int i = 0; i < nums.size(); i ++) {
            int sum = 0;
            for (int j = i; j < nums.size(); j ++) {
                sum += nums[j];
                if (sum >= target) {
                    int sublen = j - i + 1;// get子数组长度
                    // res需要比较来进行更新，故需要初始res为一个大数
                    res = res < sublen ? res : sublen;
                    break;
                }
            }
        }
        return res == max ? 0 : res;
    }
    ```

    PS: 关于C++最大值的设置: [ref](https://jimmy-shen.medium.com/0x3f3f3f3f-an-interesting-number-for-inf-f4f0a0d24124)

2、滑动窗口, 也即双指针的一种，一个for循环降低复杂度。key: **起始位置i动态的移动**
        
- 通过窗口不断调整子数组起始位置，使窗口内元素满足要求。[动图参考](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)


=== "Java"

    ```java linenums="1"
    class Solution {
        public int minSubArrayLen(int target, int[] nums) {
        int res = Integer.MAX_VALUE, sum = 0;
        for(int i = 0, j = 0; j < nums.length; j ++) {
                sum += nums[j];
                while (sum >= target) {
                    res = Math.min(res, j - i + 1);
                    sum -= nums[i ++];  
                }
        }
        return res == Integer.MAX_VALUE ? 0 : res;
        }
    }
    ```

=== "C++"

    ```c++ linenums="1" hl_lines="10"
    int minSubArrayLen(int target, vector<int>& nums) {
        int max = 0x3f3f3f3f, res = max, sum = 0;
        //i代表窗口的起始，j窗口的终止
        for(int i = 0, j = 0; j < nums.size(); j ++) {
            sum += nums[j];
            while (sum >= target) {
                int sublen = j - i + 1;
                res = res < sublen ? res : sublen;
                sum -= nums[i ++];
            }
        }
        return res == max ? 0 : res;
    }
    ```


## [59.螺旋矩阵II](https://leetcode.cn/problems/spiral-matrix-ii/)

模拟填充的过程，循环遍历，注意特殊情况和区间边界

=== "Java"

    ```java linenums="1"
    class Solution {
        public int[][] generateMatrix(int n) {
        int [][] res = new int[n][n];
        int r1 = 0, r2 = n - 1, c1 = 0, c2 = n - 1, val = 0;
        if(n == 1) res[r1][c1] = 1;
        while(r1 <= r2 && c1 <= c2) {
                for(int i = c1; i < c2; i ++) res[r1][i] = ++ val;
                for(int i = r1; i < r2; i ++) res[i][c2] = ++ val;
                for(int i = c2; i > c1; i --) res[c2][i] = ++ val;
                for(int i = r2; i > r1; i --) res[i][c1] = ++ val;
                r1 ++; c1 ++; r2 --; c2 --;
                if(r1 == r2 && c1 == c2) res[r1][c1] = ++ val;
        }
        return res;
        }
    }
    ```

=== "C++"

    ```c++ linenums="1"
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n));
        int r1 = 0, r2 = n - 1, c1 = 0, c2 = n - 1, val = 0;
        // 特殊处理n = 1
        if (n = 1) res[r1][c1] = 1;
        while (r1 <= r2 && c1 <= c2) {
            // left --> right (区间左闭右开进行处理)
            for (int i = c1; i < c2; i ++) res[r1][i] = ++ val;
            // top --> down
            for (int i = r1; i < r2; i ++) res[i][c2] = ++ val;
            // right --> left
            for (int i = c2; i > c1; i --) res[r2][i] = ++ val;
            // down --> top
            for (int i = r2; i > r1; i --) res[i][c1] = ++ val;
            r1 ++, c1 ++, r2 --, c2 --;
            // 填充中间的值
            if (r1 == r2 && c1 == c2) res[r1][c1] = ++ val;
        }
        return res;
    }
    ```

## [区间和](https://www.programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html)

> 给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和

（暴力求解会TLE）

**前缀和**：重复利用计算过的子数组之和，从而降低区间查询需要累加计算的次数

- 若求下标2至5的区间和，则对应前缀和数组`s[5]-s[1]` (两数组都从0开始)

=== "Java"

    ```java linenums="1"
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        int n = s.nextInt();
        int[] arr = new int[n];
        int[] p = new int[n];
        int t = 0;
        for (int i = 0; i < n; i++) {
            arr[i] = s.nextInt();
            t += arr[i]; p[i] = t;
        }
        while (s.hasNextInt()) {
            int a = s.nextInt(); 
            int b = s.nextInt(); //把a,b定义在上面会TLE，很奇怪
            if (a == 0) System.out.println(p[b]);
            else System.out.println(p[b] - p[a - 1]);
        }
        s.close();
    }
    ```
=== "C++"

    ```C++ linenums="1"
    #include <iostream>
    using namespace std;
    const int N = 100010;
    int a[N], s[N];
    int main() {
        int n;
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> a[i];
        for(int i = 1; i <= n; i ++) 
            s[i] = s[i - 1] + a[i];
        int a, b;
        while (cin >> a >> b) {
            if (a == 0) cout << s[b + 1] << endl;
            else cout << s[b + 1] - s[a] << endl;
        }
        return 0;
    }
    ```

PS: 注意前缀和数组和原来数组下标从哪开始! C++中`scanf/printf`要比`cout/cin`效率高

```c++
for (int i = 1;i <= n; i++) cin >> a[i]; 
for (int i = 1; i <= n; i++) s[i] = s[i - 1] + a[i];
```