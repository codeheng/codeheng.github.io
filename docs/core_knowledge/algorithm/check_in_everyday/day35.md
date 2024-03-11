---
comments: true
---

## 01背包理论

面试掌握01背包 & 完全背包就基本够了 ([背包问题示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210117171307407.png)), 重点是理解01背包问题

> 有n件物品和一个最多能背重量为w的背包。第i件物品的重量是`weight[i]`，得到的价值是`value[i]` 
> 每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大

暴力: 可以利用回溯列出所有的情况, 复杂度$O(2^n)$

**利用DP**:

1. `dp[i][j]`表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大多少([如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210110103003361.png))
2. 递归公式: `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`
   
      - 不放物品i --> 由`dp[i-1][j]`推出,即背包容量为j，里面不放物品i的最大价值
      - 放物品i --> 由`dp[i - 1][j - weight[i]]`推出

3. 初始化: 若背包容量j=0的话，则`dp[i][0]=0`; 那么dp[0][j] = ? ^^即i=0,存放编号0的物品时，各个容量的背包所能存放的最大价值^^, 此时当`j < weight[0]`时，则`dp[0][j]=0`，因为背包容量比编号0的物品重量还小。当`j >= weight[0]`时，则`dp[0][j]=value[0]`; 其他=0即可
4. 遍历顺序, 先遍历物品后背包更好理解
5. 举例: 背包最大容量4, 物品0(w:1,v:15),物品1(w:3,v:20),物品2(w:4,v:30) --> [dp数组示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210118163425129.jpg)

[题目](https://kamacoder.com/problempage.php?pid=1046)
```cpp linenums="1"
#include<bits/stdc++.h>
using namespace std;
int n, m; //n代表种类 m代表行李箱空间
void solve() {
    vector<int> weight(n, 0);//存储每件物品占的空间
    vector<int> value(n , 0); //价值
    for(int i = 0; i < n; i ++) cin >> weight[i];
    for(int i = 0; i < n; i ++) cin >> value[i];
    vector<vector<int>> dp(weight.size(), vector<int>(m + 1, 0));
    // 初始化, 因为需要用到dp[i - 1]的值
    // j < weight[0]已在上方被初始化为0
    // j >= weight[0]的值就初始化为value[0]
    for(int j = weight[0]; j <= m; j ++) dp[0][j] = value[0];
    for(int i = 1; i < weight.size(); i ++)
        for(int j = 0; j <= m; j ++) 
            if(j < weight[i]) dp[i][j] = dp[i - 1][j];
            else 
                dp[i][j] = max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i]);
    cout << dp[weight.size() - 1][m] << endl;  
}
int main() {
    while(cin >> n >> m) solve();
    return 0;
}
```

可以利用滚动数组,将二维降为一维; 如果把dp[i - 1]那一层拷贝到dp[i]上，即：`dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);`, 与其把dp[i - 1]这一层拷贝到dp[i]上，不如只用一个一维数组了

1. 在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值最大为dp[j]。
2. 递推公式 : `dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`
3. 初始化都为0
4. 遍历: 一维dp遍历的时候，背包是从大到小(倒序遍历是为了保证物品i只被放入一次)
   
    ``` cpp linenums="1"
    for(int i = 0; i < weight.size(); i++)  // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--)  // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    ```

5. 例子同上 --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210110103614769.png)

上题的另一种解法
```cpp linenums="1"
#include<bits/stdc++.h>
using namespace std;
int main() {
    int n, m;
    cin >> n >> m;
    vector<int> weight(n);
    vector<int> value(n);
    for(int i = 0; i < n; i ++) cin >> weight[i];
    for(int i = 0; i < n; i ++) cin >> value[i];
    vector<int> dp(m + 1, 0); // 创建一个动态规划数组dp，初始值为0
    for(int i = 0; i < n; i ++)
        for(int j = m; j >= weight[i]; j --)
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    cout << dp[m] << endl;
    return 0;
}
```


## [416.分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

> 给一个只包含正整数的非空数组。是否可以将数组分割成两个子集，使两个子集的元素和相等

只要找到集合里能够出现 sum / 2 的子集总和，就可以分割 (0/1背包)

- 背包的体积为sum / 2
- 背包要放入的商品（集合里的元素）weight为元素的数值，value也为元素的数值
- 背包如果正好装满，说明找到了总和为 sum / 2 的子集。
- 背包中每一个元素不可重复放入

五部曲:

1. dp[j]表示: 容量为j的背包，所背的物品价值最大可以为dp[j]
2. 01背包的递推公式为：`dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`

      - 而物品i的重量是`nums[i]`，其价值也为`nums[i]`

3. 初始化为0
4. 遍历: 若使用一维dp数组，物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历
5. 举例: 输入[1,5,11,5] --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210110104240545.png)

``` cpp linenums="1"
bool canPartition(vector<int>& nums) {
    int sum = 0;
    for(int i = 0; i < nums.size(); i ++) sum += nums[i];
    // 题目中说：每个数组中的元素不会超过 100，数组的大小不会超过 200
    // 总和不会大于20000，背包最大只需要其中一半，所以10001大小就可以了
    vector<int> dp(10001, 0);
    if(sum % 2) return false;
    int target = sum / 2;
    // 开始01背包
    for(int i = 0; i < nums.size(); i ++ )
        for(int j = target; j >= nums[i]; j --)
            dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
    if(dp[target] == target) return true;
    return false;
}
```